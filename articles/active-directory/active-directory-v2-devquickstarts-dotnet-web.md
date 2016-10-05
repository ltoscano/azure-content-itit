<properties
	pageTitle="App Web .NET v2.0 di Azure AD| Microsoft Azure"
	description="Come creare un'app Web .NET MVC che consente agli utenti di accedere sia con un account Microsoft personale sia con quello aziendale o dell'istituto di istruzione."
	services="active-directory"
	documentationCenter=".net"
	authors="dstrockis"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="dastrock"/>

# Aggiungere l'accesso a un'app Web .NET MVC

Con l'endpoint v2.0 è possibile aggiungere rapidamente l'autenticazione alle app Web con supporto per account Microsoft personali, aziendali o dell'istituto di istruzione. Nelle app Web ASP.NET, a questo scopo si usa il middleware OWIN di Microsoft incluso in .NET Framework 4.5.

> [AZURE.NOTE]
	Non tutti gli scenari e le funzionalità di Azure Active Directory sono supportati dall'endpoint 2.0. Per determinare se è necessario usare l'endpoint v2.0, leggere le informazioni sulle [limitazioni v2.0](active-directory-v2-limitations.md).

 Verrà compilata un'applicazione Web che utilizza OWIN per l'accesso dell'utente, la visualizzazione di informazioni sull'utente e la disconnessione dell'utente dall'app.
 
 ## Download Il codice per questa esercitazione è salvato [su GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet). Per seguire la procedura è possibile [scaricare la struttura dell'app come file con estensione zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) o clonare la struttura:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Al termine dell'esercitazione, verrà fornita anche l'app completata.

## Registrare un'app
Creare una nuova app in [apps.dev.microsoft.com](https://apps.dev.microsoft.com) o seguire questa [procedura dettagliata](active-directory-v2-app-registration.md). Verificare di:

- Copiare l'**ID applicazione** assegnato all'app, perché verrà richiesto a breve.
- Aggiungere la piattaforma **Web** per l'app.
- Immettere l'**URI di reindirizzamento** corretto. L'URI di reindirizzamento indica ad Azure AD dove indirizzare le risposte di autenticazione. Il valore predefinito per questa esercitazione è `https://localhost:44326/`.

## Installare e configurare l'autenticazione OWIN
In questo caso, verrà configurato il middleware OWIN per l'uso del protocollo di autenticazione OpenID Connect. OWIN verrà usato, tra le altre cose, per inviare le richieste di accesso e disconnessione, gestire la sessione dell'utente e ottenere informazioni sull'utente.

-	Per iniziare, aprire il file `web.config` nella radice del progetto e immettere i valori di configurazione dell'app nella sezione `<appSettings>`.
    -	`ida:ClientId` rappresenta l'**ID applicazione** assegnato all'app nel portale di registrazione.
    -	`ida:RedirectUri` rappresenta l'**URI di reindirizzamento** immesso nel portale.

-	Successivamente, aggiungere il middleware NuGet al progetto usando la console di Gestione pacchetti.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-	Aggiungere al progetto una OWIN Startup Class denominata `Startup.cs`. Fare clic con il pulsante destro del mouse sul progetto --> **Aggiungi** --> **Nuovo elemento** --> Cercare "OWIN". Il middleware OWIN richiamerà il metodo `Configuration(...)` all'avvio dell'app.
-	Sostituire la dichiarazione della classe con `public partial class Startup`. Parte di questa classe è già stata implementata in un altro file. Nel metodo `Configuration(...)` eseguire una chiamata a ConfigureAuth(...) per configurare l'autenticazione per l'app Web.

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
	public partial class Startup
	{
		public void Configuration(IAppBuilder app)
		{
			ConfigureAuth(app);
		}
	}
}
```

-	Aprire il file `App_Start\Startup.Auth.cs` e implementare il metodo `ConfigureAuth(...)`. I parametri forniti in `OpenIdConnectAuthenticationOptions` fungeranno da coordinate per consentire all'app di comunicare con Azure AD. È inoltre necessario impostare l'autenticazione tramite cookie: il middleware OpenID Connect usa i cookie in background.

```C#
public void ConfigureAuth(IAppBuilder app)
			 {
					 app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

					 app.UseCookieAuthentication(new CookieAuthenticationOptions());

					 app.UseOpenIdConnectAuthentication(
							 new OpenIdConnectAuthenticationOptions
							 {
									 // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
									 // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
									 // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

									 ClientId = clientId,
									 Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
									 RedirectUri = redirectUri,
									 Scope = "openid email profile",
									 ResponseType = "id_token",
									 PostLogoutRedirectUri = redirectUri,
									 TokenValidationParameters = new TokenValidationParameters
									 {
											 ValidateIssuer = false,
									 },
									 Notifications = new OpenIdConnectAuthenticationNotifications
									 {
											 AuthenticationFailed = OnAuthenticationFailed,
									 }
							 });
			 }
```

## Invio di richieste di autenticazione
L'app ora è configurata correttamente per comunicare con l'endpoint 2.0 mediante il protocollo di autenticazione OpenID Connect. OWIN ha gestito tutte le difficoltà derivanti dalla creazione dei messaggi di autenticazione, dalla convalida dei token da Azure AD e dalla gestione della sessione utente. Non resta che dare agli utenti un modo per accedere e disconnettersi.

- È possibile usare tag di autorizzazione nei controller per obbligare l'utente ad accedere prima di aprire una determinata pagina. Aprire `Controllers\HomeController.cs` e aggiungere il tag `[Authorize]` al controller About.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-	È possibile usare OWIN anche per inviare le richieste di autenticazione direttamente dal codice. Aprire `Controllers\AccountController.cs`. Nelle azioni SignIn() e SignOut() inoltrare rispettivamente le richieste di verifica e di disconnessione di OpenID Connect.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-	Ora aprire `Views\Shared\_LoginPartial.cshtml`, dove si mostreranno all'utente i collegamenti di accesso e disconnessione e si visualizzerà il nome dell'utente.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## Visualizzare le informazioni utente
Quando si autenticano gli utenti con OpenID Connect, l'endpoint 2.0 restituisce un token ID all'app che contiene [attestazioni](active-directory-v2-tokens.md#id_tokens) o asserzioni sull'utente. È possibile usare queste attestazioni per personalizzare l'app:

- Aprire il file `Controllers\HomeController.cs`. È possibile accedere alle attestazioni dell'utente nei controller tramite l'oggetto di entità di sicurezza `ClaimsPrincipal.Current`.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## Esegui

Infine compilare ed eseguire l'app. Accedere con un account Microsoft personale, aziendale o dell'istituto di istruzione e osservare come l'identità dell'utente è indicata nella barra di spostamento superiore. È ora disponibile un'app Web protetta usando protocolli standard del settore in grado di autenticare gli utenti con account personali, aziendali e dell'istituto di istruzione.

Come riferimento, l'esempio completato (senza i valori di configurazione) [è disponibile in un file con estensione zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip). In alternativa, è possibile clonarlo da GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## Passaggi successivi

È ora possibile passare ad argomenti più avanzati. È possibile consultare:

[Proteggere un'API Web con l'endpoint v2.0 >>](active-directory-devquickstarts-webapi-dotnet.md)

Per altre risorse, vedere:
- [Guida per sviluppatori v2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow: tag "azure-active-directory" >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## Ottenere aggiornamenti della sicurezza per i prodotti

È consigliabile ricevere notifiche in caso di problemi di sicurezza. A tale scopo, visitare [questa pagina](https://technet.microsoft.com/security/dd252948) e sottoscrivere gli avvisi di sicurezza.

<!---HONumber=AcomDC_0921_2016-->
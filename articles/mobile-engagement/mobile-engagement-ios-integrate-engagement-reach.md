<properties
	pageTitle="Integrazione dell'SDK di Reach per Azure Mobile Engagement in iOS | Microsoft Azure"
	description="Ultimi aggiornamenti e procedure relativi a iOS SDK per Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" /> 

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-ios"
	ms.devlang="objective-c"
	ms.topic="article"
	ms.date="09/14/2016"
	ms.author="piyushjo" /> 

#Come integrare il servizio Reach di Engagement in iOS

Prima di usare questa guida, è necessario eseguire la [procedura di integrazione descritta nel documento relativo all'integrazione di Engagement in iOS](mobile-engagement-ios-integrate-engagement.md).

Questa documentazione richiede XCode 8. Se si dipende davvero da XCode 7, allora è possibile utilizzare l'[SDK di Engagement in iOS v3.2.4](https://aka.ms/r6oouh). Esiste un bug noto nella versione precedente durante l'esecuzione su dispositivi iOS 10: le notifiche di sistema non vengono messe in atto. Per risolvere il problema è necessario implementare l'API deprecata `application:didReceiveRemoteNotification:` nell'app delegata come segue:

	- (void)application:(UIApplication*)application
	didReceiveRemoteNotification:(NSDictionary*)userInfo
	{
	    [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
	}

> [AZURE.IMPORTANT] **Questa soluzione non è consigliata** dal momento che tale comportamento può cambiare in qualsiasi aggiornamento della versione iOS imminente (anche minore), poiché questa API iOS è deprecata. È opportuno passare a XCode 8 il prima possibile.

### Abilitare l'app per la ricezione delle notifiche push Silent

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##Procedura di integrazione

### Incorporare l'SDK del servizio Reach di Engagement nel progetto iOS

-   Aggiungere l'SDK di Reach nel progetto Xcode. In Xcode scegliere **Project > Add to project** e selezionare la cartella `EngagementReach`.

### Modificare il delegato dell'applicazione

-   Nella parte superiore del file di implementazione, importare il modulo Reach di Engagement:

		[...]
		#import "AEReachModule.h"

-   All'interno del metodo `applicationDidFinishLaunching:` o `application:didFinishLaunchingWithOptions:` creare un modulo Reach e passarlo alla riga di inizializzazione di Engagement esistente:

		- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
		  AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
		  [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
		  [...]

		  return YES;
		}

-   Nella stringa **"icon.png"** inserire il nome dell'immagine da usare come icona di notifica.
-   Se si vuole impostare l'opzione *Update badge value* nelle campagne Reach oppure usare le campagne push native </SaaS/Reach API/Campaign format/Native Push>, è necessario consentire al modulo Reach di gestire autonomamente l'icona del badge. In questo modo, il modulo cancella automaticamente il badge dell'applicazione e ripristina il valore memorizzato da Engagement, ogni volta che l'applicazione viene avviata o eseguita in primo piano. È possibile effettuare questa operazione aggiungendo la riga seguente dopo il modulo di inizializzazione Reach:

		[reach setAutoBadgeEnabled:YES];

-   Se si vuole gestire il push dei dati Reach, è necessario consentire al delegato dell'applicazione di uniformarsi al protocollo `AEReachDataPushDelegate`. Aggiungere la riga seguente dopo l'inizializzazione del modulo Reach:

		[reach setDataPushDelegate:self];

-   A questo punto, è possibile implementare i metodi `onDataPushStringReceived:` e `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` nel delegato dell'applicazione:

		-(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
		{
		   NSLog(@"String data push message with category <%@> received: %@", category, body);
		   return YES;
		}

		-(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
		{
		   NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
		   // Do something useful with decodedBody like updating an image view
		   return YES;
		}

### Categoria

Il parametro category è facoltativo quando si crea una campagna per il push dei dati e consente di filtrare i push dei dati. Questo parametro è utile quando si vuole effettuare il push di vari tipi di dati `Base64` al fine di identificare il tipo prima dell'analisi.

**L'applicazione è ora pronta per ricevere e visualizzare i contenuti Reach.**

##Come ricevere annunci e sondaggi in qualsiasi momento

Engagement consente di inviare notifiche Reach agli utenti finali, in qualsiasi momento, usando Apple Push Notification Service.

Per attivare questa funzionalità, è necessario preparare l'applicazione all'uso delle notifiche push Apple e modificare il delegato dell'applicazione.

### Preparare l'applicazione per le notifiche push Apple

Seguire la procedura in: [How to Prepare your Application for Apple Push Notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6) (Come preparare l'applicazione per le notifiche push Apple)

### Aggiungere il codice del client adeguato

*A questo punto, l'applicazione dovrebbe avere un certificato push registrato di Apple nel front-end di Engagement.*

Nel caso in cui non sia stato già fatto, è necessario registrare l'applicazione affinché possa ricevere notifiche push. Aggiungere la riga seguente all'avvio dell'applicazione (in genere, in `application:didFinishLaunchingWithOptions:`):

	if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
	  	[application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
	  	[application registerForRemoteNotifications];
	}
	else {
	  	[application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
	}

In seguito, è necessario fornire a Engagement il token del dispositivo restituito dai server Apple. Questa operazione viene effettuata nel delegato dell'applicazione, all'interno del metodo `application:didRegisterForRemoteNotificationsWithDeviceToken:`:

	- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
	{
	    [[EngagementAgent shared] registerDeviceToken:deviceToken];
	}

Infine, è necessario comunicare all'SDK di Engagement il momento in cui l'applicazione riceve una notifica remota. A tale scopo, chiamare il metodo `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` nel delegato dell'applicazione:

	- (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
	{
		[[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
	}

> [AZURE.NOTE] Il metodo qui sopra è stato introdotto in iOS 7. Se la destinazione è una versione precedente di iOS, assicurarsi di implementare il metodo `application:didReceiveRemoteNotification:` nel delegato dell'applicazione e chiamare `applicationDidReceiveRemoteNotification` su EngagementAgent passando nil anziché l'argomento `handler`:

	- (void)application:(UIApplication*)application
	didReceiveRemoteNotification:(NSDictionary*)userInfo
	{
		[[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
	}

> [AZURE.IMPORTANT] Per impostazione predefinita, Reach di Engagement controlla completionHandler. Se si desidera rispondere manualmente al blocco `handler` nel codice, è possibile passare nil per l'argomento `handler` e controllare il completamento del blocco. Vedere il tipo `UIBackgroundFetchResult` per un elenco di valori possibili.


### Esempio completo

Di seguito, è riportato un esempio completo sull'integrazione:

	#pragma mark -
	#pragma mark Application lifecycle

	- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
	{
	  /* Reach module */
	  AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
	  [reach setAutoBadgeEnabled:YES];

	  /* Engagement initialization */
	  [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
	  [[EngagementAgent shared] setPushDelegate:self];

	  /* Views */
	  [window addSubview:[tabBarController view]];
	  [window makeKeyAndVisible];

	  [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
	  return YES;
	}

	- (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
	{
	  [[EngagementAgent shared] registerDeviceToken:deviceToken];
	}

	- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
	{
		[[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
	}

### Se si dispone di un'implementazione di UNUserNotificationCenterDelegate:

L'SDK dispone inoltre della propria implementazione del protocollo UNUserNotificationCenterDelegate. Viene utilizzata dall'SDK per monitorare il ciclo di vita delle notifiche di Engagement sui dispositivi che eseguono iOS 10 o versioni successive. Se l'SDK rileva il delegato non utilizza la sua implementazione poiché può esistere un solo delegato UNUserNotificationCenter per applicazione. Ciò significa che è necessario aggiungere la logica di Engagement al proprio delegato.

A questo scopo è possibile procedere in due modi:

Inoltrando semplicemente le chiamate del delegato all'SDK

	#import <UIKit/UIKit.h>
	#import "EngagementAgent.h"
	#import <UserNotifications/UserNotifications.h>


	@interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
	@end

	@implementation MyAppDelegate

	- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
	{
	  // Your own logic.

	  [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
	}

	- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
	{
	  // Your own logic.

	  [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
	}
	@end

oppure ereditandola dalla classe `AEUserNotificationHandler`

	#import "AEUserNotificationHandler.h"
	#import "EngagementAgent.h"

	@interface CustomUserNotificationHandler :AEUserNotificationHandler
	@end

	@implementation CustomUserNotificationHandler

	- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
	{
	  // Your own logic.

	  [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
	}

	- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
	{
	  // Your own logic.

	  [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
	}

	@end

> [AZURE.NOTE] È possibile determinare se una notifica proviene o no da Engagement passando il suo dizionario `userInfo` al metodo della classe dell'agente `isEngagementPushPayload:`.

##Come personalizzare le campagne

### Notifiche

Esistono due tipi di notifiche: quelle di sistema e quelle in-app.

Le notifiche di sistema vengono gestite da iOS e non possono essere personalizzate.

Le notifiche in-app sono costituite da una visualizzazione che viene aggiunta in modo dinamico alla finestra corrente dell'applicazione. Si tratta di una sovrimpressione di notifica. Le sovrimpressioni delle notifiche sono ideali per un'integrazione rapida, poiché non richiedono alcuna modifica delle visualizzazioni dell'applicazione.

#### Layout

Per modificare l'aspetto delle notifiche in-app, è possibile personalizzare il file `AENotificationView.xib`, purché vengano conservati i valori dei tag e i tipi delle visualizzazioni secondarie esistenti.

Per impostazione predefinita, le notifiche in-app vengono visualizzate nella parte inferiore dello schermo. Se si preferisce visualizzarle nella parte superiore dello schermo, modificare il file `AENotificationView.xib` fornito e la proprietà `AutoSizing` della visualizzazione principale in modo che venga mantenuta nella parte superiore della visualizzazione sovrapposta.

#### Categorie

La modifica del layout fornito comporta un cambiamento nell'aspetto di tutte le notifiche. Le categorie consentono di definire diversi aspetti assegnati (comportamenti) delle notifiche. Quando si crea una campagna Reach, è possibile specificare una categoria. Tenere presente che le categorie consentono di personalizzare annunci e sondaggi, come descritto successivamente nel documento.

Per registrare un gestore di categoria per le notifiche, è necessario aggiungere una chiamata una volta inizializzato il modulo di portata.

	AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
	[reach registerNotifier:myNotifier forCategory:@"my_category"];
	...

`myNotifier` deve essere un'istanza di un oggetto conforme al protocollo `AENotifier`.

È possibile implementare i metodi del protocollo autonomamente o scegliere di implementare di nuovo la classe `AEDefaultNotifier` esistente, che esegue già la maggior parte delle operazioni.

Ad esempio, se si desidera ridefinire la visualizzazione delle notifiche per una categoria specifica, è possibile attenersi a questo esempio:

	#import "AEDefaultNotifier.h"
	#import "AENotificationView.h"
	@interface MyNotifier : AEDefaultNotifier
	@end

	@implementation MyNotifier

	-(NSString*)nibNameForCategory:(NSString*)category
	{
	  return "MyNotificationView";
	}

	@end

In questo semplice esempio di categoria si presuppone che nel pacchetto dell'applicazione sia presente un file denominato `MyNotificationView.xib`. Se il metodo non è in grado di trovare un file `.xib` corrispondente, la notifica non viene visualizzata ed Engagement restituisce un messaggio nella console.

Il file con estensione nib fornito deve rispettare le regole seguenti:

-   Deve includere soltanto una visualizzazione.
-   Le visualizzazioni secondarie devono essere di un tipo analogo a quelle presenti all'interno del file nib `AENotificationView.xib`.
-   Le visualizzazioni secondarie devono disporre degli stessi tag di quelle presenti nel file nib `AENotificationView.xib`.

> [AZURE.TIP] È sufficiente copiare il file nib fornito, denominato `AENotificationView.xib`, e apportare modifiche a tale file. Fare però attenzione, poiché la visualizzazione all'interno del file nib è associata alla classe `AENotificationView`. Questa classe ha ridefinito il metodo `layoutSubViews` in modo da spostare e ridimensionare le visualizzazioni secondarie in base al contesto. È possibile sostituirla con una classe `UIView` o con una classe di visualizzazione personalizzata.

Se si desidera personalizzare ulteriormente le notifiche (ad esempio, caricare la visualizzazione direttamente dal codice), si consiglia di fare riferimento al codice di origine fornito e alla documentazione relativa alle classi di `Protocol ReferencesDefaultNotifier` e `AENotifier`.

Tenere presente che è possibile usare la stessa notifica per più categorie.

Inoltre, è possibile ridefinire il componente di notifica predefinito nel modo seguente:

	AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
	[reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### Gestione delle notifiche

Quando si usa la categoria predefinita, alcuni metodi del ciclo di vita vengono chiamati sull'oggetto `AEReachContent` per segnalare le statistiche e aggiornare lo stato della campagna:

-   Quando la notifica viene visualizzata in un'applicazione, il metodo `displayNotification` (per la segnalazione delle statistiche) viene chiamato da `AEReachModule` se `handleNotification:` restituisce `YES`.
-   Se la notifica viene ignorata, viene chiamato il metodo `exitNotification`, vengono segnalate le statistiche e le campagne successive possono essere elaborate.
-   Se la notifica viene selezionata, viene chiamato `actionNotification`, vengono segnalate le statistiche e viene eseguita l'azione associata.

Se l'implementazione di `AENotifier` ignora il comportamento predefinito, è necessario chiamare i metodi del ciclo di vita autonomamente. Negli esempi seguenti vengono descritte alcune situazioni nelle quali viene ignorato il comportamento predefinito:

-   Il metodo `AEDefaultNotifier` non è stato esteso, ad esempio la gestione delle categorie è stata implementata da zero.
-   È stato eseguito l'override di `prepareNotificationView:forContent:`. Assicurarsi di mappare almeno `onNotificationActioned` o `onNotificationExited` a uno dei controlli dell'interfaccia utente.

> [AZURE.WARNING] Se `handleNotification:` genera un'eccezione, i contenuti vengono eliminati e viene chiamato il metodo `drop`. L'evento viene segnalato nelle modifiche ed è possibile procedere con l'elaborazione delle campagne successive.

#### Includere la notifica come parte di una visualizzazione esistente

Le sovrimpressioni rappresentano un metodo ottimale per eseguire integrazioni rapide; talvolta, però, non sono convenienti e possono produrre effetti collaterali indesiderati.

Se il sistema di sovrimpressione di alcune visualizzazioni non è soddisfacente, è possibile personalizzarlo.

È possibile decidere di includere un layout per le notifiche personalizzato nelle visualizzazioni esistenti. A tale scopo, sono disponibili due stili di implementazione:

1.  Aggiungere la visualizzazione delle notifiche usando lo strumento di creazione delle interfacce

	-   Aprire lo *strumento di creazione delle interfacce*
	-   Posizionare un oggetto `UIView` della dimensione di 320x60 (768x60 su iPad) nell'area in cui si vuole visualizzare la notifica
	-   Impostare il tag della visualizzazione sul valore seguente: **36822491**

2.  Aggiungere la visualizzazione delle notifiche a livello di programmazione. È sufficiente aggiungere il codice seguente, quando viene inizializzata la visualizzazione:

		UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
		notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
		[self.view addSubview:notificationView];

La macro `NOTIFICATION_AREA_VIEW_TAG` è disponibile in `AEDefaultNotifier.h`.

> [AZURE.NOTE] Il componente di notifica predefinito rileva automaticamente che il layout per le notifiche è incluso nella visualizzazione e non vi aggiunge una sovrimpressione.

### Annunci e sondaggi

#### Layout

È possibile modificare i file `AEDefaultAnnouncementView.xib` e `AEDefaultPollView.xib`, purché vengano mantenuti i valori dei tag e i tipi di visualizzazioni secondarie esistenti.

#### Categorie

##### Layout alternativi

In modo analogo alle notifiche, è possibile impostare la categoria di una campagna in modo che disponga di layout alternativi per gli annunci e i sondaggi.

Per creare la categoria di un annuncio, è necessario estendere **AEAnnouncementViewController** e registrarlo quando viene inizializzato il modulo Reach:

	AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
	[reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Ogni volta che un utente selezionerà la notifica di un annuncio con categoria "my\_category", il controller di visualizzazione registrato (in questo caso `MyCustomAnnouncementViewController`) verrà inizializzato chiamando il metodo `initWithAnnouncement:`. In questo modo, la visualizzazione verrà aggiunta alla finestra corrente dell'applicazione.

Durante l'implementazione della classe `AEAnnouncementViewController`, è necessario leggere la proprietà `announcement` al fine di inizializzare le visualizzazioni secondarie. Fare riferimento all'esempio seguente, nel quale due etichette vengono inizializzate mediante le proprietà `title` e `body` della classe `AEReachAnnouncement`:

	-(void)loadView
	{
	    [super loadView];

	    UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
	    titleLabel.font = [UIFont systemFontOfSize:32.0];
	    titleLabel.text = self.announcement.title;

	    UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
	    bodyLabel.font = [UIFont systemFontOfSize:24.0];
	    bodyLabel.text = self.announcement.body;

	    [self.view addSubview:titleLabel];
	    [self.view addSubview:bodyLabel];
	}

Se non si vuole caricare le visualizzazioni autonomamente, ma solo riutilizzare il layout predefinito della visualizzazione di annunci, è sufficiente impostare il controller di visualizzazione personalizzato in modo che estenda la classe `AEDefaultAnnouncementViewController` fornita. In tal caso, duplicare il file nib `AEDefaultAnnouncementView.xib` e rinominarlo in modo che possa essere caricato dal controller di visualizzazione personalizzato. Nel caso di un controller denominato `CustomAnnouncementViewController`, assegnare il nome `CustomAnnouncementView.xib` al file nib.

Per sostituire la categoria predefinita degli annunci, è sufficiente registrare il controller di visualizzazione personalizzato per la categoria definita in `kAEReachDefaultCategory`:

	[reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

È possibile personalizzare i sondaggi nello stesso modo:

	AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
	[reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Questa volta, l'oggetto `MyCustomPollViewController` fornito deve estendere `AEPollViewController`. In alternativa, è possibile scegliere di eseguire l'estensione dal controller predefinito: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Ricordare di chiamare il metodo `action` (`submitAnswers:` per i controller di visualizzazione sondaggi personalizzati) o il metodo `exit` prima di ignorare il controller di visualizzazione. In caso contrario, le statistiche non verranno inviate (vale a dire, nessuna analisi sulla campagna) e le successive campagne non verranno notificate fino al riavvio del processo dell'applicazione.

##### Esempio di implementazione

In questa implementazione la visualizzazione di annuncio personalizzata viene caricata da un file xib esterno.

Come per la personalizzazione avanzata delle notifiche, si consiglia di esaminare il codice sorgente dell'implementazione standard.

`CustomAnnouncementViewController.h`

	//Interface
	@interface CustomAnnouncementViewController : AEAnnouncementViewController {
	  UILabel* titleLabel;
	  UITextView* descTextView;
	  UIWebView* htmlWebView;
	  UIButton* okButton;
	  UIButton* cancelButton;
	}

	@property (nonatomic, retain) IBOutlet UILabel* titleLabel;
	@property (nonatomic, retain) IBOutlet UITextView* descTextView;
	@property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
	@property (nonatomic, retain) IBOutlet UIButton* okButton;
	@property (nonatomic, retain) IBOutlet UIButton* cancelButton;

	-(IBAction)okButtonClicked:(id)sender;
	-(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m` 

	//Implementation
	@implementation CustomAnnouncementViewController
	@synthesize titleLabel;
	@synthesize descTextView;
	@synthesize htmlWebView;
	@synthesize okButton;
	@synthesize cancelButton;

	-(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
	{
	  self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
	  if (self != nil) {
	    self.announcement = anAnnouncement;
	  }
	  return self;
	}

	- (void) dealloc
	{
	  [titleLabel release];
	  [descTextView release];
	  [htmlWebView release];
	  [okButton release];
	  [cancelButton release];
	  [super dealloc];
	}

	- (void)viewDidLoad {
	  [super viewDidLoad];

	  /* Init announcement title */
	  titleLabel.text = self.announcement.title;

	  /* Init announcement body */
	  if(self.announcement.type == AEAnnouncementTypeHtml)
	  {
	    titleLabel.hidden = YES;
	    htmlWebView.hidden = NO;
	    [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
	  }
	  else
	  {
	    titleLabel.hidden = NO;
	    htmlWebView.hidden = YES;
	    descTextView.text = self.announcement.body;
	  }

	  /* Set action button label */
	  if([self.announcement.actionLabel length] > 0)
	    [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

	  /* Set exit button label */
	  if([self.announcement.exitLabel length] > 0)
	    [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
	}

	#pragma mark Actions

	-(IBAction)okButtonClicked:(id)sender
	{
	    [self action];
	}

	-(IBAction)cancelButtonClicked:(id)sender
	{
	    [self exit];
	}

	@end

<!---HONumber=AcomDC_0921_2016-->
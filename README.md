
>Before getting start with payment, make sure [App360 SDK](https://github.com/app360/app360-wp-sdk) is that integrated.

#Payment
##Payment flow

In order to secure payment flow, your application might choose to integrate with our SDK on both client-side and server-side, in which case the payment flow is depict in the following diagram:
![enter image description here](https://camo.githubusercontent.com/0ace684f85e4b82195f68c92b6f00e71e33609c7/687474703a2f2f7777772e77656273657175656e63656469616772616d732e636f6d2f6367692d62696e2f63647261773f6c7a3d64476c306247556755474635625756756443427a5a5846315a57356a5a516f4b523246745a53302d5530524c4f67415543584a6c6358566c633351674b444570436c4e455379302d523246745a546f6756484a68626e4e6859335270623234676157517349484e3059585231637977675957317664573530494367794b514247423064686257556763325679646d56794f69427a5a57356b494851414d41746b5958526849475a7663694270626e4e775a51424a4269677a4146384c4144494a41436f4d61575141595167734948567a5a584a66615751674b445141624159415a5163416754384859574e72494368495646525141494561427941794d44417049436731414230504149455944574e76626d5a70636d30674b44594145784d41464173334b5126733d726f7365)

 1. The application (client-side) calls payment API from the SDK, optionally with a custom payload (documented below).    
 2. The SDK returns transaction id and other details (if available) 
 3. The application  client sends transaction data to its server awaiting confirmation
 4. SDK server calls a pre-registered endpoint of the application server
    to notify about transaction status when it completes
    - Note that there is no guarantee about the order of (3) and (4) (i.e. (4) may happen before (3)) 
 5. Application server acknowledges SDK server's call by
    responding with HTTP status code 200.
 6. Application server validates the transaction based on the information it has (transaction ID,
    payload, etc.)
 7. Application server confirms/notifies game client
    about the status of the transaction

Note:

To register your application's server endpoint, go to [https://developers.app360.vn/](https://developers.app360.vn/) set Payment callback endpoint in application details page, Information tab.
Before using any payment methods, the application must first initialize the SDK. See section SDK Initialization above.



## Using payment classes



 Create a new instance of MOGPaymentSDK
```csharp
// create a new instance
MOGPaymentSDK paymentSDK = new MOGPaymentSDK();
```
###To make a **banking** charging request
```csharp
// amount: amount of money for charge
MOGBankingTransaction bankTransaction = await paymentSDK.MakeBankingTransaction("amount", "payload");
 
if (bankTransaction.IsSuccess)
{
    // To see card transactoin detail                
    Debug.WriteLine("\n` Payment Url: {0}", bankTransaction.PaymentUrl);
}
else
{
    // To see error
    Debug.WriteLine("Error: {0}", bankTransaction.ErrorInfo);
}
```

### To make a **SMS** charging request
```csharp
var smsTransaction = (MOGSMSTransaction)await paymentSDK.GetSMSSyntax("yourpayload", "1000, 15000, 500", Telecom.VIETTEL);
if (smsTransaction.IsSuccess)
{
   // To see SMS detail
   foreach (var item in smsTransaction.SMSItems)
   {
      Debug.WriteLine("\n` SMS: {0}\n` To: {1}\n` Amount: {2}", item.Text, item.To, item.Amount);
   }
}
else
{
   // To see error
   Debug.WriteLine("Error: {0}", smsTransaction.ErrorInfo);
}
```
###To make a **Card** charging request

```csharp
// vendor: defined in MOGCardVendor,
// you also create a new Vendor by using new Vendor("NewVendorName");
// card_code: the code of the phone card
// serial: The serial of the phone card
MOGPhoneCardTransaction phonecardTransaction = await paymentSDK.MakePhoneCardTransaction( <vendor>, "<card_code>", "<serial>", "<payload>");
if (phonecard.IsSuccess)
{
    // To see card transaction detail                
    Debug.WriteLine("\n` Thẻ: {0}\n` PIN: {1}\n` SERIAL: {2}", phonecardTransaction.CardType, phonecardTransaction.Pin, phonecardTransaction.Serial);
}
else
{
    // To see error
    Debug.WriteLine("Error: {0}", phonecardTransaction.ErrorInfo);
}
```

`// payload: Some custom string you want to send to your server when transaction finished.`

## Checking transaction status

To check the status of a transaction
```
// transaction_id: Id of transaction
 MOGStatusTransaction status = await paymentSDK.CheckStatusOfTransaction("<transaction_id>");
```
 

















































































































































































































































































































































































































































































































































22


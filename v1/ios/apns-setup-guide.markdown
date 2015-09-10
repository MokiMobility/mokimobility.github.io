---
layout: page
title:  "APNs Setup Guide"
tags: mobile, ios
---

Before you can integrate your application with MokiManage, you need to go through the process of configuring it to receive push notifications from Apple (APNs). You must have an active [iOS Developer Program](https://developer.apple.com/programs/ios/) membership in order to complete these steps.

The end result of this process are private SSL certificates that are stored securely for you on [mokimanage.com](https://www.mokimanage.com). These certificates allow MokiManage to send APNs messages to your application through the MokiManage platform on your behalf.

## Configure your Application

![Apple Interface](/assets/select_app_id.png)

If you have not previously created your application identifier, click the + button and complete the form. Make sure the Push Notifications checkbox is selected.

![Apple Interface](/assets/app_creation.png)

After you have selected your application identifier, you will see that there are two settings for push notifications that display yellow or green status icons.

![Apple Interface](/assets/push_notification_status.png)

Click the Edit button. If you are not able to select the Edit button, then you may not be the Team Agent or Account Admin. You have to be a Team Agent or Admin in order to configure APNs.

In order to enable Development or Production APNs messaging in your application, you need to create the appropriate certificate. Click the Create Certificate button under the Development SSL section to begin that process.

![Apple Interface](/assets/configure_certs.png)

You have to create a Certificate Signing Request (CSR) in order to create the APNs certificate. You create this CSR using Keychain Access on your Mac. Follow the instructions provided in the Developer Portal to create the CSR and click Continue.

![Apple Interface](/assets/create_csr.png)

Upload the CSR to the Developer Portal and click Generate

![Apple Interface](/assets/upload_csr.png)

Download the certificate that is created and double click it to open in Keychain Access.

![Apple Interface](/assets/download_cert.png)

In Keychain Access, the certificate will be displayed in the My Certificates section with a label of Apple Development IOS Push Services: <app-identifier> (it will be labeled Apple Production IOS Push Services for production certs).

![Apple Interface](/assets/keychain.png)

Once the certificate is installed, you need to export both the cert and the associated private key to a .p12 file. Expand the cert so that the private key is displayed and select both items. Right click and select "Export 2 items...". Save the .p12 file and provide a password when prompted.

![Apple Interface](/assets/export_cert.png)

![Apple Interface](/assets/password_box.png)

In order for the MokiManage platform to utilize your APNs cert you have just created, it needs to be in .pem format. Open a Terminal window and navigate to the directory where you saved your .p12 file.

Convert your .p12 to a .pem using the following OpenSSL command (replace your_cert.p12 with the name of the your .p12 file and your_cert.pem with whatever you want to name your .pem file) and enter the password you selected when you exported the .p12:

	openssl pkcs12 -in your_cert.p12 -out your_cert.pem -nodes -clcerts
	
Upload the .pem file to the Developer APNS Cert item inside of the Developer Tools section of mokimanage.com

![Apple Interface](/assets/cert_upload.png)

Repeat this same process for the production SSL cert and upload it to the Store APNS Cert item.

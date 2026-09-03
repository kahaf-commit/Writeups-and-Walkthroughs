# Delivery Techniques

# Email Delivery

It is a common method to use in order to send the payload by sending a

email with a link or attachment. For more info, visit [here (opens in new tab)](https://attack.mitre.org/techniques/T1566/001/).
 This method attaches a malicious file that could be the type we 
mentioned earlier. The goal is to convince the victim to visit a 
malicious website or download and run the malicious file to gain initial
 access to the victim's network or host.

The red teamers should have their own infrastructure for phishing 
purposes. Depending on the red team engagement requirement, it requires 
setting up various options within the email server, including DomainKeys
 Identified Mail (

), Sender Policy Framework (), and

Pointer (PTR) record.

The red teamers could also use third-party email services such as 
Google Gmail, Outlook, Yahoo, and others with good reputations.

Another interesting method would be to use a compromised email 
account within a company to send phishing emails within the company or 
to others. The compromised email could be hacked by phishing or by other
 techniques such as password spraying attacks.

![A laptop connected with various devices and networks](https://cdn-images.tryhackme.com/user-uploads/5c549500924ec576f953d9fc/room-content/08a3f660501cf5171277534e40aa96b8.png)

### Web Delivery

Another method is hosting malicious payloads on a web server 
controlled by the red teamers. The web server has to follow the security
 guidelines such as a clean record and reputation of its domain name and

(Transport Layer Security) certificate. For more information, visit [here (opens in new tab)](https://attack.mitre.org/techniques/T1189/).

This method includes other techniques such as

the victim to visit or download the malicious file. A URL shortener could be helpful when using this method.

In this method, other techniques can be combined and used. The 
attacker can take advantage of zero-day exploits such as exploiting 
vulnerable software like Java or browsers to use them in phishing emails
 or web delivery techniques to gain access to the victim machine.

![A laptop and a smartphone with a USB cable](https://cdn-images.tryhackme.com/user-uploads/5c549500924ec576f953d9fc/room-content/ff8ca3c104fa32e30603ecf97ee0d72e.png)

### 

# USB Delivery

This
 method requires the victim to plug in the malicious USB physically. 
This method could be effective and useful at conferences or events where
 the adversary can distribute the USB. For more information about USB 
delivery, visit [here (opens in new tab)](https://attack.mitre.org/techniques/T1091/).

Often,
 organizations establish strong policies such as disabling USB usage 
within their organization environment for security purposes. While other
 organizations allow it in the target environment.

Common USB attacks used to weaponize USB devices include [Rubber Ducky (opens in new tab)](https://shop.hak5.org/products/usb-rubber-ducky-deluxe) and [USBHarpoon (opens in new tab)](https://www.minitool.com/news/usbharpoon.html), charging USB cable, such as [O.MG Cable (opens in new tab)](https://shop.hak5.org/products/omg-cable).
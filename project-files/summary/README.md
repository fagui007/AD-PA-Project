## Summary
This lab demonstrated how an enterprise network can be securely segmented and managed using a **Next-Generation Firewall** integrated with **Active Directory services**.

- The **PA-440 NGFW** was used to separate the environment into Clients, Servers, and Management zones, with policies controlling traffic between them. We also used a Configuration zone to configure the firewall from our host PC.
- A **Windows Server 2025 Active Directory infrastructure** was deployed, including **DNS, DHCP, and a two-tier PKI** using AD CS.
- The **Issuing CA** was leveraged to issue a trusted certificate for the firewall, enabling **SSL decryption** so that encrypted traffic could be inspected securely.
- The **Windows-based User-ID Agent** was configured to integrate AD user and group identity into the firewall, enabling **identity-based security policies**.  
- Testing confirmed that clients could authenticate, obtain IP/DNS settings via DHCP, receive certificates, and access resources according to security rules tied to their AD group memberships.  

This environment provides a **realistic enterprise-grade lab** that demonstrates the integration of NGFWs with AD services and PKI. It can be further extended to include advanced topics such as VPNs, RADIUS/NPS integration, and endpoint security.

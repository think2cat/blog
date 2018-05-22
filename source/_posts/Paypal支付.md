---
title: Paypal支付 - ExpressCheckout接口文档
tags:
  - paypal
  - soap
  - wsdl
  - xml
  - 网银
id: 1025
categories:
  - Web
abbrlink: 7d8cd342
date: 2008-05-14 19:01:37
---

最近公司有个网站需要用到paypal支付接口，看了下支付方式，ExpressCheckout比较符合需求，于是写了这文档

作者：SK猫
创建时间：5/5/2008 2:47 PM
最后修改：5/5/2008 4:09 PM

<span style="font-size: large">一、&nbsp;支付流程</span>

![](/images/2008/05/14_200805141816580111_12766.gif)

&nbsp;

<span style="font-size: large">二、&nbsp;SOAP运作方式</span>

<span style="font-size: medium">1.&nbsp;SetExpressCheckoutRequest</span>

**<span style="font-size: small">Request内容</span>**

    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
    &lt;SOAP-ENV:Envelope xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
    :SOAP-ENC=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot;
    :SOAP-ENV=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot;
    :xsd=&quot;http://www.w3.org/2001/XMLSchema&quot;
    ENV:encodingStyle=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot;&gt;
    &nbsp;&lt;SOAP-ENV:Header&gt;
    &nbsp;&nbsp;&lt;RequesterCredentials xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Credentials xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Username&gt;gavin2_1207532001_biz_api1.hotmail.com&lt;/Username&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Password&gt;xxxxxxxxxx&lt;/Password&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Signature&gt;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&lt;/Signature&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Subject/&gt;
    &nbsp;&nbsp;&nbsp;&lt;/Credentials&gt;
    &nbsp;&nbsp;&lt;/RequesterCredentials&gt;
    &nbsp;&lt;/SOAP-ENV:Header&gt;
    &nbsp;&lt;SOAP-ENV:Body&gt;
    &nbsp;&nbsp;&lt;SetExpressCheckoutReq xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;SetExpressCheckoutRequest xsi:type=&quot;ns:SetExpressCheckoutRequestType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Version xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;xsd:string&quot;&gt;1.0&lt;/Version&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;SetExpressCheckoutRequestDetails xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;OrderTotal xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; currencyID=&quot;USD&quot; xsi:type=&quot;cc:BasicAmountType&quot;&gt;58.00&lt;/OrderTotal&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;OrderDescription&gt;2008-5-5 14:58:34&lt;/OrderDescription&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;ReturnURL xsi:type=&quot;xsd:string&quot;&gt;http://www.21ido.com/paypal/returnorder.asp&lt;/ReturnURL&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;CancelURL xsi:type=&quot;xsd:string&quot;&gt;http://www.21ido.com/paypal/cancelorder.asp&lt;/CancelURL&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/SetExpressCheckoutRequestDetails&gt;
    &nbsp;&nbsp;&nbsp;&lt;/SetExpressCheckoutRequest&gt;
    &nbsp;&nbsp;&nbsp;&lt;PaymentAction&gt;Sale&lt;/PaymentAction&gt;
    &nbsp;&nbsp;&lt;/SetExpressCheckoutReq&gt;
    &nbsp;&lt;/SOAP-ENV:Body&gt;
    &lt;/SOAP-ENV:Envelope&gt;`</pre>

    **<span style="font-size: small">参数介绍：</span>**

*   Head里面的Username、Password、Signature为API验证信息，需在paypal获取，下同
*   OrderTotal：currencyID=&quot;USD&quot;表示美元，其它货币单位参照paypal文档（PP_APIReference.pdf），数值必须带2位小数
*   OrderDescription：商品描述，可选
*   ReturnURL：返回地址，购买者填写帐号信息确认后会返回该地址
*   CancelURL：当购买者取消订单后返回的地址

    <span style="color: #ff0000">OrderTotal在第三步要用到，需传递或保存Session</span>

    **<span style="font-size: small">Response内容</span>**
    <pre>`&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
    &lt;SOAP-ENV:Envelope xmlns:SOAP-ENV=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:SOAP-ENC=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot; xmlns:xsd=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:xs=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:cc=&quot;urn:ebay:apis:CoreComponentTypes&quot; xmlns:wsu=&quot;http://schemas.xmlsoap.org/ws/2002/07/utility&quot; xmlns:saml=&quot;urn:oasis:names:tc:SAML:1.0:assertion&quot; xmlns:ds=&quot;http://www.w3.org/2000/09/xmldsig#&quot; xmlns:market=&quot;urn:ebay:apis:Market&quot; xmlns:auction=&quot;urn:ebay:apis:Auction&quot; xmlns:sizeship=&quot;urn:ebay:api:PayPalAPI/sizeship.xsd&quot; xmlns:ship=&quot;urn:ebay:apis:ship&quot; xmlns:skype=&quot;urn:ebay:apis:skype&quot; xmlns:wsse=&quot;http://schemas.xmlsoap.org/ws/2002/12/secext&quot; xmlns:ebl=&quot;urn:ebay:apis:eBLBaseComponents&quot; xmlns:ns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&lt;SOAP-ENV:Header&gt;
    &nbsp;&nbsp;&lt;Security xmlns=&quot;http://schemas.xmlsoap.org/ws/2002/12/secext&quot; xsi:type=&quot;wsse:SecurityType&quot;/&gt;
    &nbsp;&nbsp;&lt;RequesterCredentials xmlns=&quot;urn:ebay:api:PayPalAPI&quot; xsi:type=&quot;ebl:CustomSecurityHeaderType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Credentials xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;ebl:UserIdPasswordType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Username xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Password xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Signature xsi:type=&quot;xs:string&quot;&gt;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&lt;/Signature&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Subject xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&lt;/Credentials&gt;
    &nbsp;&nbsp;&lt;/RequesterCredentials&gt;
    &nbsp;&lt;/SOAP-ENV:Header&gt;
    &nbsp;&lt;SOAP-ENV:Body id=&quot;_0&quot;&gt;
    &nbsp;&nbsp;&lt;SetExpressCheckoutResponse xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Timestamp xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;2008-05-04T03:22:02Z&lt;/Timestamp&gt;
    &nbsp;&nbsp;&nbsp;&lt;Ack xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;Success&lt;/Ack&gt;
    &nbsp;&nbsp;&nbsp;&lt;CorrelationID xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;134cf1c8d6faf&lt;/CorrelationID&gt;
    &nbsp;&nbsp;&nbsp;&lt;Version xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;1.000000&lt;/Version&gt;
    &nbsp;&nbsp;&nbsp;&lt;Build xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;548868&lt;/Build&gt;
    &nbsp;&nbsp;&nbsp;&lt;Token xsi:type=&quot;ebl:ExpressCheckoutTokenType&quot;&gt;EC-4F193608SG720004S&lt;/Token&gt;
    &nbsp;&nbsp;&lt;/SetExpressCheckoutResponse&gt;
    &nbsp;&lt;/SOAP-ENV:Body&gt;
    &lt;/SOAP-ENV:Envelope&gt;`</pre>

    **当Ack值为Seccess时为请求成功，下同**
    取回 Token ，并重定向到paypal，例如
    <u>[https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&amp;token](https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&amp;token)</u><u>= </u><span style="color: #3366ff"><u>EC-4F193608SG720004S</u>
    </span>

    &nbsp;

    <span style="font-size: medium">2.&nbsp;GetExpressCheckoutDetailsRequest</span>

    用户在paypal填写完帐户信息后，点continue 后将跳转回之前设置的ReturnUrl地址，并带上参数，例如
    <span style="color: #3366ff"><u>http://www.21ido.com/paypal/return.asp?token=EC-4F193608SG720004S&amp;PayerID=WS8QWH5MLUK6C</u></span>
    当ReturnUrl地址本身已带参数时，如 <span style="color: #3366ff">return.asp?id=212121</span>，跳转地址将为
    <span style="color: #3366ff">return.asp?id=212121&amp; token=EC-4F193608SG720004S&amp;PayerID=WS8QWH5MLUK6C</span>

    此时需调用GetExpressCheckoutDetailsRequest方法取回用户填写的信息，例如帐号、地址

    **<span style="font-size: small">Request内容</span>**
    <pre>`&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8
    &quot;?&gt;
    &lt;SOAP-ENV:Envelope xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot; xmlns:SOAP-ENC=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot; xmlns:SOAP-ENV=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:xsd=&quot;http://www.w3.org/2001/XMLSchema&quot; SOAP-ENV:encodingStyle=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot;&gt;
    &nbsp;&lt;SOAP-ENV:Header&gt;
    &nbsp;&nbsp;&lt;RequesterCredentials xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Credentials xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Username&gt;gavin2_1207532001_biz_api1.hotmail.com&lt;/Username&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Password&gt;xxxxxxxxxx&lt;/Password&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Signature&gt;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&lt;/Signature&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Subject/&gt;
    &nbsp;&nbsp;&nbsp;&lt;/Credentials&gt;
    &nbsp;&nbsp;&lt;/RequesterCredentials&gt;
    &nbsp;&lt;/SOAP-ENV:Header&gt;
    &nbsp;&lt;SOAP-ENV:Body&gt;
    &nbsp;&nbsp;&lt;GetExpressCheckoutDetailsReq xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;GetExpressCheckoutDetailsRequest xsi:type=&quot;ns:SetExpressCheckoutRequestType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Version xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;xsd:string&quot;&gt;1.0&lt;/Version&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Token&gt;EC-15361037RR099961G&lt;/Token&gt;
    &nbsp;&nbsp;&nbsp;&lt;/GetExpressCheckoutDetailsRequest&gt;
    &nbsp;&nbsp;&lt;/GetExpressCheckoutDetailsReq&gt;
    &nbsp;&lt;/SOAP-ENV:Body&gt;
    &lt;/SOAP-ENV:Envelope&gt;`</pre>

    <span style="color: #ff0000">其中Token 为paypal传回的数值，传回的PayerID此步无作用，但下一步确认支付时需要提交给paypal，可用参数方式或session方式传递给下一步</span>

    **<span style="font-size: small">Response内容</span>**
    <pre>`&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
    &lt;SOAP-ENV:Envelope xmlns:SOAP-ENV=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:SOAP-ENC=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot; xmlns:xsd=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:xs=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:cc=&quot;urn:ebay:apis:CoreComponentTypes&quot; xmlns:wsu=&quot;http://schemas.xmlsoap.org/ws/2002/07/utility&quot; xmlns:saml=&quot;urn:oasis:names:tc:SAML:1.0:assertion&quot; xmlns:ds=&quot;http://www.w3.org/2000/09/xmldsig#&quot; xmlns:market=&quot;urn:ebay:apis:Market&quot; xmlns:auction=&quot;urn:ebay:apis:Auction&quot; xmlns:sizeship=&quot;urn:ebay:api:PayPalAPI/sizeship.xsd&quot; xmlns:ship=&quot;urn:ebay:apis:ship&quot; xmlns:skype=&quot;urn:ebay:apis:skype&quot; xmlns:wsse=&quot;http://schemas.xmlsoap.org/ws/2002/12/secext&quot; xmlns:ebl=&quot;urn:ebay:apis:eBLBaseComponents&quot; xmlns:ns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&lt;SOAP-ENV:Header&gt;
    &nbsp;&nbsp;&lt;Security xmlns=&quot;http://schemas.xmlsoap.org/ws/2002/12/secext&quot; xsi:type=&quot;wsse:SecurityType&quot;/&gt;
    &nbsp;&nbsp;&lt;RequesterCredentials xmlns=&quot;urn:ebay:api:PayPalAPI&quot; xsi:type=&quot;ebl:CustomSecurityHeaderType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Credentials xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;ebl:UserIdPasswordType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Username xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Password xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Signature xsi:type=&quot;xs:string&quot;&gt;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&lt;/Signature&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Subject xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&lt;/Credentials&gt;
    &nbsp;&nbsp;&lt;/RequesterCredentials&gt;
    &nbsp;&lt;/SOAP-ENV:Header&gt;
    &nbsp;&lt;SOAP-ENV:Body id=&quot;_0&quot;&gt;
    &nbsp;&nbsp;&lt;GetExpressCheckoutDetailsResponse xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Timestamp xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;2008-05-05T07:37:23Z&lt;/Timestamp&gt;
    &nbsp;&nbsp;&nbsp;&lt;Ack xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;Success&lt;/Ack&gt;
    &nbsp;&nbsp;&nbsp;&lt;CorrelationID xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;a8bf169692ef0&lt;/CorrelationID&gt;
    &nbsp;&nbsp;&nbsp;&lt;Version xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;1.000000&lt;/Version&gt;
    &nbsp;&nbsp;&nbsp;&lt;Build xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;548868&lt;/Build&gt;
    &nbsp;&nbsp;&nbsp;&lt;GetExpressCheckoutDetailsResponseDetails xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;ebl:GetExpressCheckoutDetailsResponseDetailsType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Token xsi:type=&quot;ebl:ExpressCheckoutTokenType&quot;&gt;EC-15361037RR099961G&lt;/Token&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerInfo xsi:type=&quot;ebl:PayerInfoType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Payer xsi:type=&quot;ebl:EmailAddressType&quot;&gt;gavin2_1207532695_per@hotmail.com&lt;/Payer&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerID xsi:type=&quot;ebl:UserIDType&quot;&gt;WS8QWH5MLUK6C&lt;/PayerID&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerStatus xsi:type=&quot;ebl:PayPalUserStatusCodeType&quot;&gt;verified&lt;/PayerStatus&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerName xsi:type=&quot;ebl:PersonNameType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Salutation xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;FirstName xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;Test&lt;/FirstName&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;MiddleName xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;LastName xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;User&lt;/LastName&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Suffix xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/PayerName&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerCountry xsi:type=&quot;ebl:CountryCodeType&quot;&gt;US&lt;/PayerCountry&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerBusiness xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Address xsi:type=&quot;ebl:AddressType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Name xsi:type=&quot;xs:string&quot;&gt;Test User&lt;/Name&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Street1 xsi:type=&quot;xs:string&quot;&gt;1 Main St&lt;/Street1&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Street2 xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;CityName xsi:type=&quot;xs:string&quot;&gt;San Jose&lt;/CityName&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;StateOrProvince xsi:type=&quot;xs:string&quot;&gt;CA&lt;/StateOrProvince&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Country xsi:type=&quot;ebl:CountryCodeType&quot;&gt;US&lt;/Country&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;CountryName/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PostalCode xsi:type=&quot;xs:string&quot;&gt;95131&lt;/PostalCode&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;AddressOwner xsi:type=&quot;ebl:AddressOwnerCodeType&quot;&gt;PayPal&lt;/AddressOwner&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;AddressStatus xsi:type=&quot;ebl:AddressStatusCodeType&quot;&gt;Confirmed&lt;/AddressStatus&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/Address&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/PayerInfo&gt;
    &nbsp;&nbsp;&nbsp;&lt;/GetExpressCheckoutDetailsResponseDetails&gt;
    &nbsp;&nbsp;&lt;/GetExpressCheckoutDetailsResponse&gt;
    &nbsp;&lt;/SOAP-ENV:Body&gt;
    &lt;/SOAP-ENV:Envelope&gt;`</pre>

    购买者信息，参数具体介绍参照SOAP文档（PP_APIReference.pdf）

    &nbsp;

    <span style="font-size: medium">3.&nbsp;DoExpressCheckoutPaymentRequest
    </span>

    确认支付，这一步返回Completed的话表示钱已经打到商家帐户

    **<span style="font-size: small">Request内容</span>**
    <pre>`&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
    &lt;SOAP-ENV:Envelope xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot;
    :SOAP-ENC=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot;
    :SOAP-ENV=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot;
    :xsd=&quot;http://www.w3.org/2001/XMLSchema&quot;
    ENV:encodingStyle=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot;&gt;
    &nbsp;&lt;SOAP-ENV:Header&gt;
    &nbsp;&nbsp;&lt;RequesterCredentials xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Credentials xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Username&gt;gavin2_1207532001_biz_api1.hotmail.com&lt;/Username&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Password&gt;xxxxxxxxxx&lt;/Password&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Signature&gt;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&lt;/Signature&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Subject/&gt;
    &nbsp;&nbsp;&nbsp;&lt;/Credentials&gt;
    &nbsp;&nbsp;&lt;/RequesterCredentials&gt;
    &nbsp;&lt;/SOAP-ENV:Header&gt;
    &nbsp;&lt;SOAP-ENV:Body&gt;
    &nbsp;&nbsp;&lt;DoExpressCheckoutPaymentReq xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;DoExpressCheckoutPaymentRequest xsi:type=&quot;ns:SetExpressCheckoutRequestType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Version xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;xsd:string&quot;&gt;1.0&lt;/Version&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;DoExpressCheckoutPaymentRequestDetails xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;Token xsi:type=&quot;ebl:ExpressCheckoutTokenType&quot;&gt;EC-15361037RR099961G&lt;/Token&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PayerID&gt;WS8QWH5MLUK6C&lt;/PayerID&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PaymentAction&gt;Sale&lt;/PaymentAction&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PaymentDetails xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;OrderTotal xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; currencyID=&quot;USD&quot; xsi:type=&quot;cc:BasicAmountType&quot;&gt;29.00&lt;/OrderTotal&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;/PaymentDetails&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/DoExpressCheckoutPaymentRequestDetails&gt;
    &nbsp;&nbsp;&nbsp;&lt;/DoExpressCheckoutPaymentRequest&gt;
    &nbsp;&nbsp;&lt;/DoExpressCheckoutPaymentReq&gt;
    &nbsp;&lt;/SOAP-ENV:Body&gt;
    &lt;/SOAP-ENV:Envelope&gt;`</pre>

    Token、PayerID、PaymentAction、OrderTotal等各参数均为之前提交或获取到的

    <span style="font-size: small">Response内容</span>
    <pre>`&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
    &lt;SOAP-ENV:Envelope xmlns:SOAP-ENV=&quot;http://schemas.xmlsoap.org/soap/envelope/&quot; xmlns:SOAP-ENC=&quot;http://schemas.xmlsoap.org/soap/encoding/&quot; xmlns:xsi=&quot;http://www.w3.org/2001/XMLSchema-instance&quot; xmlns:xsd=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:xs=&quot;http://www.w3.org/2001/XMLSchema&quot; xmlns:cc=&quot;urn:ebay:apis:CoreComponentTypes&quot; xmlns:wsu=&quot;http://schemas.xmlsoap.org/ws/2002/07/utility&quot; xmlns:saml=&quot;urn:oasis:names:tc:SAML:1.0:assertion&quot; xmlns:ds=&quot;http://www.w3.org/2000/09/xmldsig#&quot; xmlns:market=&quot;urn:ebay:apis:Market&quot; xmlns:auction=&quot;urn:ebay:apis:Auction&quot; xmlns:sizeship=&quot;urn:ebay:api:PayPalAPI/sizeship.xsd&quot; xmlns:ship=&quot;urn:ebay:apis:ship&quot; xmlns:skype=&quot;urn:ebay:apis:skype&quot; xmlns:wsse=&quot;http://schemas.xmlsoap.org/ws/2002/12/secext&quot; xmlns:ebl=&quot;urn:ebay:apis:eBLBaseComponents&quot; xmlns:ns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&lt;SOAP-ENV:Header&gt;
    &nbsp;&nbsp;&lt;Security xmlns=&quot;http://schemas.xmlsoap.org/ws/2002/12/secext&quot; xsi:type=&quot;wsse:SecurityType&quot;/&gt;
    &nbsp;&nbsp;&lt;RequesterCredentials xmlns=&quot;urn:ebay:api:PayPalAPI&quot; xsi:type=&quot;ebl:CustomSecurityHeaderType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Credentials xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;ebl:UserIdPasswordType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Username xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Password xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Signature xsi:type=&quot;xs:string&quot;&gt;xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx&lt;/Signature&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Subject xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&lt;/Credentials&gt;
    &nbsp;&nbsp;&lt;/RequesterCredentials&gt;
    &nbsp;&lt;/SOAP-ENV:Header&gt;
    &nbsp;&lt;SOAP-ENV:Body id=&quot;_0&quot;&gt;
    &nbsp;&nbsp;&lt;DoExpressCheckoutPaymentResponse xmlns=&quot;urn:ebay:api:PayPalAPI&quot;&gt;
    &nbsp;&nbsp;&nbsp;&lt;Timestamp xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;2008-05-05T07:56:38Z&lt;/Timestamp&gt;
    &nbsp;&nbsp;&nbsp;&lt;Ack xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;Success&lt;/Ack&gt;
    &nbsp;&nbsp;&nbsp;&lt;CorrelationID xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;167cc5fc71e87&lt;/CorrelationID&gt;
    &nbsp;&nbsp;&nbsp;&lt;Version xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;1.000000&lt;/Version&gt;
    &nbsp;&nbsp;&nbsp;&lt;Build xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot;&gt;548868&lt;/Build&gt;
    &nbsp;&nbsp;&nbsp;&lt;DoExpressCheckoutPaymentResponseDetails xmlns=&quot;urn:ebay:apis:eBLBaseComponents&quot; xsi:type=&quot;ebl:DoExpressCheckoutPaymentResponseDetailsType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;Token xsi:type=&quot;ebl:ExpressCheckoutTokenType&quot;&gt;EC-60499907HA323812X&lt;/Token&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;PaymentInfo xsi:type=&quot;ebl:PaymentInfoType&quot;&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;TransactionID&gt;84V49327HA333602X&lt;/TransactionID&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;ParentTransactionID xsi:type=&quot;ebl:TransactionId&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;ReceiptID/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;TransactionType xsi:type=&quot;ebl:PaymentTransactionCodeType&quot;&gt;express-checkout&lt;/TransactionType&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PaymentType xsi:type=&quot;ebl:PaymentCodeType&quot;&gt;instant&lt;/PaymentType&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PaymentDate xsi:type=&quot;xs:dateTime&quot;&gt;2008-05-05T07:56:37Z&lt;/PaymentDate&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;GrossAmount xsi:type=&quot;cc:BasicAmountType&quot; currencyID=&quot;USD&quot;&gt;55.00&lt;/GrossAmount&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;FeeAmount xsi:type=&quot;cc:BasicAmountType&quot; currencyID=&quot;USD&quot;&gt;1.90&lt;/FeeAmount&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;TaxAmount xsi:type=&quot;cc:BasicAmountType&quot; currencyID=&quot;USD&quot;&gt;0.00&lt;/TaxAmount&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;ExchangeRate xsi:type=&quot;xs:string&quot;/&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PaymentStatus xsi:type=&quot;ebl:PaymentStatusCodeType&quot;&gt;Completed&lt;/PaymentStatus&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;PendingReason xsi:type=&quot;ebl:PendingStatusCodeType&quot;&gt;none&lt;/PendingReason&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;ReasonCode xsi:type=&quot;ebl:ReversalReasonCodeType&quot;&gt;none&lt;/ReasonCode&gt;
    &nbsp;&nbsp;&nbsp;&nbsp;&lt;/PaymentInfo&gt;
    &nbsp;&nbsp;&nbsp;&lt;/DoExpressCheckoutPaymentResponseDetails&gt;
    &nbsp;&nbsp;&lt;/DoExpressCheckoutPaymentResponse&gt;
    &nbsp;&lt;/SOAP-ENV:Body&gt;
    &lt;/SOAP-ENV:Envelope&gt;

**<span style="font-size: small">参数介绍：</span>**

*   TransactionID&nbsp;&nbsp;paypal流水号
*   GrossAmount&nbsp;&nbsp;收取金额
*   FeeAmount&nbsp;&nbsp;手续费，从商家扣除
*   PaymentStatus&nbsp;&nbsp;支付状态<
/li>    <li>其它参数请参考SOAP文档

&nbsp;

<span style="font-size: large">三、&nbsp;附录</span>

<span style="font-size: medium">1.&nbsp;各接口地址</span>

Environment&nbsp; &nbsp;Authentication&nbsp; &nbsp;Calling&nbsp; &nbsp;&nbsp;&nbsp;Endpoint
Live &nbsp;&nbsp;&nbsp;API Certificate &nbsp;Name-Value Pair &nbsp;[https://api.paypal.com/nvp](https://api.paypal.com/nvp)
Live &nbsp;&nbsp;&nbsp;API Signature &nbsp;Name-Value Pair &nbsp;[https://api-3t.paypal.com/nvp](https://api-3t.paypal.com/nvp)
Live &nbsp;&nbsp;&nbsp;API Certificate &nbsp;SOAP &nbsp;&nbsp;&nbsp;[https://api.paypal.com/2.0/](https://api.paypal.com/2.0/)
Live &nbsp;&nbsp;&nbsp;API Signature &nbsp;SOAP &nbsp;&nbsp;&nbsp;[https://api-3t.paypal.com/2.0/](https://api-3t.paypal.com/2.0/)
Sandbox &nbsp;&nbsp;API Certificate &nbsp;Name-Value Pair &nbsp;[https://api.sandbox.paypal.com/nvp](https://api.sandbox.paypal.com/nvp)
Sandbox &nbsp;&nbsp;API Signature &nbsp;Name-Value Pair &nbsp;[https://api-3t.sandbox.paypal.com/nvp](https://api-3t.sandbox.paypal.com/nvp)
Sandbox &nbsp;&nbsp;API Certificate &nbsp;SOAP &nbsp;&nbsp;&nbsp;[https://api.sandbox.paypal.com/2.0/](https://api.sandbox.paypal.com/2.0/)
Sandbox &nbsp;&nbsp;API Signature &nbsp;SOAP &nbsp;&nbsp;&nbsp;[https://api-3t.sandbox.paypal.com/2.0](https://api-3t.sandbox.paypal.com/2.0)

<span style="font-size: x-small">**Paypal重定向地址**</span>
Live&nbsp;&nbsp;https://www. Paypal.com/cgi-bin/webscr?cmd=_express-checkout&amp;token=
SandBox&nbsp;https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&amp;token=value

<span style="font-size: medium">2.&nbsp;网址</span>

开发者中心 [https://developer.paypal.com/](https://developer.paypal.com/)
各种源码下载 [https://www.paypal.com/IntegrationCenter/ic_downloads.html](https://www.paypal.com/IntegrationCenter/ic_downloads.html)
API Error [https://www.paypal.com/IntegrationCenter/ic_api-errors.html](https://www.paypal.com/IntegrationCenter/ic_api-errors.html)

<span style="font-size: medium">3.&nbsp;测试帐号</span>

Developer主帐号
[https://developer.paypal.com/](https://developer.paypal.com/)
Username&nbsp;[gavin2026@hotmail.com](mailto:gavin2026@hotmail.com)
Password&nbsp;&nbsp;xxxxxx

**Seller**
Username&nbsp;[gavin2_1207532001_biz@hotmail.com](mailto:gavin2_1207532001_biz@hotmail.com)
Password&nbsp;&nbsp;xxxxxx
**Buye**
Username&nbsp;[gavin2_1207532695_per@hotmail.com](mailto:gavin2_1207532695_per@hotmail.com)
Password&nbsp;&nbsp;xxxxxx

API验证信息
Username&nbsp;gavin2_1207532001_biz_api1.hotmail.com
Password&nbsp;&nbsp;xxxxxx
Signature&nbsp; &nbsp;xxxxxx
&nbsp;
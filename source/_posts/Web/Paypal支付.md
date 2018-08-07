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

创建时间：5/5/2008 2:47 PM
最后修改：5/5/2008 4:09 PM

## 一、支付流程

![](/images/2008/05/14_200805141816580111_12766.gif)


## 二、SOAP运作方式

### 1.SetExpressCheckoutRequest

**Request内容**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" :SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" :SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" :xsd="http://www.w3.org/2001/XMLSchema" ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <SOAP-ENV:Header>
        <RequesterCredentials
            xmlns="urn:ebay:api:PayPalAPI">
            <Credentials
                xmlns="urn:ebay:apis:eBLBaseComponents">
                <Username>gavin2_1207532001_biz_api1.hotmail.com</Username>
                <Password>xxxxxxxxxx</Password>
                <Signature>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Signature>
                <Subject/>
            </Credentials>
        </RequesterCredentials>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <SetExpressCheckoutReq
            xmlns="urn:ebay:api:PayPalAPI">
            <SetExpressCheckoutRequest xsi:type="ns:SetExpressCheckoutRequestType">
                <Version
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="xsd:string">1.0
                </Version>
                <SetExpressCheckoutRequestDetails
                    xmlns="urn:ebay:apis:eBLBaseComponents">
                    <OrderTotal
                        xmlns="urn:ebay:apis:eBLBaseComponents" currencyID="USD" xsi:type="cc:BasicAmountType">58.00
                    </OrderTotal>
                    <OrderDescription>2008-5-5 14:58:34</OrderDescription>
                    <ReturnURL xsi:type="xsd:string">http://www.21ido.com/paypal/returnorder.asp</ReturnURL>
                    <CancelURL xsi:type="xsd:string">http://www.21ido.com/paypal/cancelorder.asp</CancelURL>
                </SetExpressCheckoutRequestDetails>
            </SetExpressCheckoutRequest>
            <PaymentAction>Sale</PaymentAction>
        </SetExpressCheckoutReq>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```


**参数介绍：**

*   Head里面的Username、Password、Signature为API验证信息，需在paypal获取，下同
*   OrderTotal：currencyID=&quot;USD&quot;表示美元，其它货币单位参照paypal文档（PP_APIReference.pdf），数值必须带2位小数
*   OrderDescription：商品描述，可选
*   ReturnURL：返回地址，购买者填写帐号信息确认后会返回该地址
*   CancelURL：当购买者取消订单后返回的地址

OrderTotal在第三步要用到，需传递或保存Session

**Response内容**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
    xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:cc="urn:ebay:apis:CoreComponentTypes"
    xmlns:wsu="http://schemas.xmlsoap.org/ws/2002/07/utility"
    xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
    xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
    xmlns:market="urn:ebay:apis:Market"
    xmlns:auction="urn:ebay:apis:Auction"
    xmlns:sizeship="urn:ebay:api:PayPalAPI/sizeship.xsd"
    xmlns:ship="urn:ebay:apis:ship"
    xmlns:skype="urn:ebay:apis:skype"
    xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/12/secext"
    xmlns:ebl="urn:ebay:apis:eBLBaseComponents"
    xmlns:ns="urn:ebay:api:PayPalAPI">
    <SOAP-ENV:Header>
        <Security
            xmlns="http://schemas.xmlsoap.org/ws/2002/12/secext" xsi:type="wsse:SecurityType"/>
            <RequesterCredentials
                xmlns="urn:ebay:api:PayPalAPI" xsi:type="ebl:CustomSecurityHeaderType">
                <Credentials
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="ebl:UserIdPasswordType">
                    <Username xsi:type="xs:string"/>
                    <Password xsi:type="xs:string"/>
                    <Signature xsi:type="xs:string">xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Signature>
                    <Subject xsi:type="xs:string"/>
                </Credentials>
            </RequesterCredentials>
        </SOAP-ENV:Header>
        <SOAP-ENV:Body id="_0">
            <SetExpressCheckoutResponse
                xmlns="urn:ebay:api:PayPalAPI">
                <Timestamp
                    xmlns="urn:ebay:apis:eBLBaseComponents">2008-05-04T03:22:02Z
                </Timestamp>
                <Ack
                    xmlns="urn:ebay:apis:eBLBaseComponents">Success
                </Ack>
                <CorrelationID
                    xmlns="urn:ebay:apis:eBLBaseComponents">134cf1c8d6faf
                </CorrelationID>
                <Version
                    xmlns="urn:ebay:apis:eBLBaseComponents">1.000000
                </Version>
                <Build
                    xmlns="urn:ebay:apis:eBLBaseComponents">548868
                </Build>
                <Token xsi:type="ebl:ExpressCheckoutTokenType">EC-4F193608SG720004S</Token>
            </SetExpressCheckoutResponse>
        </SOAP-ENV:Body>
    </SOAP-ENV:Envelope>
```

**当Ack值为Seccess时为请求成功，下同**

取回 Token ，并重定向到paypal，例如
`https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&token=EC-4F193608SG720004S`

### 2.GetExpressCheckoutDetailsRequest

用户在paypal填写完帐户信息后，点continue 后将跳转回之前设置的ReturnUrl地址，并带上参数，例如
`http://www.21ido.com/paypal/return.asp?token=EC-4F193608SG720004S&PayerID=WS8QWH5MLUK6C`


当ReturnUrl地址本身已带参数时，如 `return.asp?id=212121`，跳转地址将为
`return.asp?id=212121&token=EC-4F193608SG720004S&PayerID=WS8QWH5MLUK6C`

此时需调用GetExpressCheckoutDetailsRequest方法取回用户填写的信息，例如帐号、地址

**Request内容**

```xml
<?xml version="1.0" encoding="UTF-8 "?>
<SOAP-ENV:Envelope
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
    xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema" SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <SOAP-ENV:Header>
        <RequesterCredentials
            xmlns="urn:ebay:api:PayPalAPI">
            <Credentials
                xmlns="urn:ebay:apis:eBLBaseComponents">
                <Username>gavin2_1207532001_biz_api1.hotmail.com</Username>
                <Password>xxxxxxxxxx</Password>
                <Signature>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Signature>
                <Subject/>
            </Credentials>
        </RequesterCredentials>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <GetExpressCheckoutDetailsReq
            xmlns="urn:ebay:api:PayPalAPI">
            <GetExpressCheckoutDetailsRequest xsi:type="ns:SetExpressCheckoutRequestType">
                <Version
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="xsd:string">1.0
                </Version>
                <Token>EC-15361037RR099961G</Token>
            </GetExpressCheckoutDetailsRequest>
        </GetExpressCheckoutDetailsReq>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

其中Token 为paypal传回的数值，传回的PayerID此步无作用，但下一步确认支付时需要提交给paypal，可用参数方式或session方式传递给下一步

**Response内容**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
    xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:cc="urn:ebay:apis:CoreComponentTypes"
    xmlns:wsu="http://schemas.xmlsoap.org/ws/2002/07/utility"
    xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
    xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
    xmlns:market="urn:ebay:apis:Market"
    xmlns:auction="urn:ebay:apis:Auction"
    xmlns:sizeship="urn:ebay:api:PayPalAPI/sizeship.xsd"
    xmlns:ship="urn:ebay:apis:ship"
    xmlns:skype="urn:ebay:apis:skype"
    xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/12/secext"
    xmlns:ebl="urn:ebay:apis:eBLBaseComponents"
    xmlns:ns="urn:ebay:api:PayPalAPI">
    <SOAP-ENV:Header>
        <Security
            xmlns="http://schemas.xmlsoap.org/ws/2002/12/secext" xsi:type="wsse:SecurityType"/>
            <RequesterCredentials
                xmlns="urn:ebay:api:PayPalAPI" xsi:type="ebl:CustomSecurityHeaderType">
                <Credentials
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="ebl:UserIdPasswordType">
                    <Username xsi:type="xs:string"/>
                    <Password xsi:type="xs:string"/>
                    <Signature xsi:type="xs:string">xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Signature>
                    <Subject xsi:type="xs:string"/>
                </Credentials>
            </RequesterCredentials>
        </SOAP-ENV:Header>
        <SOAP-ENV:Body id="_0">
            <GetExpressCheckoutDetailsResponse
                xmlns="urn:ebay:api:PayPalAPI">
                <Timestamp
                    xmlns="urn:ebay:apis:eBLBaseComponents">2008-05-05T07:37:23Z
                </Timestamp>
                <Ack
                    xmlns="urn:ebay:apis:eBLBaseComponents">Success
                </Ack>
                <CorrelationID
                    xmlns="urn:ebay:apis:eBLBaseComponents">a8bf169692ef0
                </CorrelationID>
                <Version
                    xmlns="urn:ebay:apis:eBLBaseComponents">1.000000
                </Version>
                <Build
                    xmlns="urn:ebay:apis:eBLBaseComponents">548868
                </Build>
                <GetExpressCheckoutDetailsResponseDetails
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="ebl:GetExpressCheckoutDetailsResponseDetailsType">
                    <Token xsi:type="ebl:ExpressCheckoutTokenType">EC-15361037RR099961G</Token>
                    <PayerInfo xsi:type="ebl:PayerInfoType">
                        <Payer xsi:type="ebl:EmailAddressType">gavin2_1207532695_per@hotmail.com</Payer>
                        <PayerID xsi:type="ebl:UserIDType">WS8QWH5MLUK6C</PayerID>
                        <PayerStatus xsi:type="ebl:PayPalUserStatusCodeType">verified</PayerStatus>
                        <PayerName xsi:type="ebl:PersonNameType">
                            <Salutation
                                xmlns="urn:ebay:apis:eBLBaseComponents"/>
                                <FirstName
                                    xmlns="urn:ebay:apis:eBLBaseComponents">Test
                                </FirstName>
                                <MiddleName
                                    xmlns="urn:ebay:apis:eBLBaseComponents"/>
                                    <LastName
                                        xmlns="urn:ebay:apis:eBLBaseComponents">User
                                    </LastName>
                                    <Suffix
                                        xmlns="urn:ebay:apis:eBLBaseComponents"/>
                                    </PayerName>
                                    <PayerCountry xsi:type="ebl:CountryCodeType">US</PayerCountry>
                                    <PayerBusiness xsi:type="xs:string"/>
                                    <Address xsi:type="ebl:AddressType">
                                        <Name xsi:type="xs:string">Test User</Name>
                                        <Street1 xsi:type="xs:string">1 Main St</Street1>
                                        <Street2 xsi:type="xs:string"/>
                                        <CityName xsi:type="xs:string">San Jose</CityName>
                                        <StateOrProvince xsi:type="xs:string">CA</StateOrProvince>
                                        <Country xsi:type="ebl:CountryCodeType">US</Country>
                                        <CountryName/>
                                        <PostalCode xsi:type="xs:string">95131</PostalCode>
                                        <AddressOwner xsi:type="ebl:AddressOwnerCodeType">PayPal</AddressOwner>
                                        <AddressStatus xsi:type="ebl:AddressStatusCodeType">Confirmed</AddressStatus>
                                    </Address>
                                </PayerInfo>
                            </GetExpressCheckoutDetailsResponseDetails>
                        </GetExpressCheckoutDetailsResponse>
                    </SOAP-ENV:Body>
                </SOAP-ENV:Envelope>
```

购买者信息，参数具体介绍参照SOAP文档（PP_APIReference.pdf）


### 3.DoExpressCheckoutPaymentRequest

确认支付，这一步返回Completed的话表示钱已经打到商家帐户

**Request内容**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" :SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" :SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" :xsd="http://www.w3.org/2001/XMLSchema" ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <SOAP-ENV:Header>
        <RequesterCredentials
            xmlns="urn:ebay:api:PayPalAPI">
            <Credentials
                xmlns="urn:ebay:apis:eBLBaseComponents">
                <Username>gavin2_1207532001_biz_api1.hotmail.com</Username>
                <Password>xxxxxxxxxx</Password>
                <Signature>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Signature>
                <Subject/>
            </Credentials>
        </RequesterCredentials>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
        <DoExpressCheckoutPaymentReq
            xmlns="urn:ebay:api:PayPalAPI">
            <DoExpressCheckoutPaymentRequest xsi:type="ns:SetExpressCheckoutRequestType">
                <Version
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="xsd:string">1.0
                </Version>
                <DoExpressCheckoutPaymentRequestDetails
                    xmlns="urn:ebay:apis:eBLBaseComponents">
                    <Token xsi:type="ebl:ExpressCheckoutTokenType">EC-15361037RR099961G</Token>
                    <PayerID>WS8QWH5MLUK6C</PayerID>
                    <PaymentAction>Sale</PaymentAction>
                    <PaymentDetails
                        xmlns="urn:ebay:apis:eBLBaseComponents">
                        <OrderTotal
                            xmlns="urn:ebay:apis:eBLBaseComponents" currencyID="USD" xsi:type="cc:BasicAmountType">29.00
                        </OrderTotal>
                    </PaymentDetails>
                </DoExpressCheckoutPaymentRequestDetails>
            </DoExpressCheckoutPaymentRequest>
        </DoExpressCheckoutPaymentReq>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

Token、PayerID、PaymentAction、OrderTotal等各参数均为之前提交或获取到的

**Response内容**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
    xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xs="http://www.w3.org/2001/XMLSchema"
    xmlns:cc="urn:ebay:apis:CoreComponentTypes"
    xmlns:wsu="http://schemas.xmlsoap.org/ws/2002/07/utility"
    xmlns:saml="urn:oasis:names:tc:SAML:1.0:assertion"
    xmlns:ds="http://www.w3.org/2000/09/xmldsig#"
    xmlns:market="urn:ebay:apis:Market"
    xmlns:auction="urn:ebay:apis:Auction"
    xmlns:sizeship="urn:ebay:api:PayPalAPI/sizeship.xsd"
    xmlns:ship="urn:ebay:apis:ship"
    xmlns:skype="urn:ebay:apis:skype"
    xmlns:wsse="http://schemas.xmlsoap.org/ws/2002/12/secext"
    xmlns:ebl="urn:ebay:apis:eBLBaseComponents"
    xmlns:ns="urn:ebay:api:PayPalAPI">
    <SOAP-ENV:Header>
        <Security
            xmlns="http://schemas.xmlsoap.org/ws/2002/12/secext" xsi:type="wsse:SecurityType"/>
            <RequesterCredentials
                xmlns="urn:ebay:api:PayPalAPI" xsi:type="ebl:CustomSecurityHeaderType">
                <Credentials
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="ebl:UserIdPasswordType">
                    <Username xsi:type="xs:string"/>
                    <Password xsi:type="xs:string"/>
                    <Signature xsi:type="xs:string">xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx</Signature>
                    <Subject xsi:type="xs:string"/>
                </Credentials>
            </RequesterCredentials>
        </SOAP-ENV:Header>
        <SOAP-ENV:Body id="_0">
            <DoExpressCheckoutPaymentResponse
                xmlns="urn:ebay:api:PayPalAPI">
                <Timestamp
                    xmlns="urn:ebay:apis:eBLBaseComponents">2008-05-05T07:56:38Z
                </Timestamp>
                <Ack
                    xmlns="urn:ebay:apis:eBLBaseComponents">Success
                </Ack>
                <CorrelationID
                    xmlns="urn:ebay:apis:eBLBaseComponents">167cc5fc71e87
                </CorrelationID>
                <Version
                    xmlns="urn:ebay:apis:eBLBaseComponents">1.000000
                </Version>
                <Build
                    xmlns="urn:ebay:apis:eBLBaseComponents">548868
                </Build>
                <DoExpressCheckoutPaymentResponseDetails
                    xmlns="urn:ebay:apis:eBLBaseComponents" xsi:type="ebl:DoExpressCheckoutPaymentResponseDetailsType">
                    <Token xsi:type="ebl:ExpressCheckoutTokenType">EC-60499907HA323812X</Token>
                    <PaymentInfo xsi:type="ebl:PaymentInfoType">
                        <TransactionID>84V49327HA333602X</TransactionID>
                        <ParentTransactionID xsi:type="ebl:TransactionId"/>
                        <ReceiptID/>
                        <TransactionType xsi:type="ebl:PaymentTransactionCodeType">express-checkout</TransactionType>
                        <PaymentType xsi:type="ebl:PaymentCodeType">instant</PaymentType>
                        <PaymentDate xsi:type="xs:dateTime">2008-05-05T07:56:37Z</PaymentDate>
                        <GrossAmount xsi:type="cc:BasicAmountType" currencyID="USD">55.00</GrossAmount>
                        <FeeAmount xsi:type="cc:BasicAmountType" currencyID="USD">1.90</FeeAmount>
                        <TaxAmount xsi:type="cc:BasicAmountType" currencyID="USD">0.00</TaxAmount>
                        <ExchangeRate xsi:type="xs:string"/>
                        <PaymentStatus xsi:type="ebl:PaymentStatusCodeType">Completed</PaymentStatus>
                        <PendingReason xsi:type="ebl:PendingStatusCodeType">none</PendingReason>
                        <ReasonCode xsi:type="ebl:ReversalReasonCodeType">none</ReasonCode>
                    </PaymentInfo>
                </DoExpressCheckoutPaymentResponseDetails>
            </DoExpressCheckoutPaymentResponse>
        </SOAP-ENV:Body>
    </SOAP-ENV:Envelope>
```

**参数介绍：**

*   TransactionIDpaypal流水号
*   GrossAmount收取金额
*   FeeAmount手续费，从商家扣除
*   PaymentStatus支付状态
*   其它参数请参考SOAP文档



## 三、附录

### 1.各接口地址

* Environment Authentication Calling Endpoint Live API Certificate Name-Value Pair [https://api.paypal.com/nvp](https://api.paypal.com/nvp)
* Live API Signature Name-Value Pair [https://api-3t.paypal.com/nvp](https://api-3t.paypal.com/nvp)
* Live API Certificate SOAP [https://api.paypal.com/2.0/](https://api.paypal.com/2.0/)
* Live API Signature SOAP [https://api-3t.paypal.com/2.0/](https://api-3t.paypal.com/2.0/)
* Sandbox API Certificate Name-Value Pair [https://api.sandbox.paypal.com/nvp](https://api.sandbox.paypal.com/nvp)
* Sandbox API Signature Name-Value Pair [https://api-3t.sandbox.paypal.com/nvp](https://api-3t.sandbox.paypal.com/nvp)
* Sandbox API Certificate SOAP [https://api.sandbox.paypal.com/2.0/](https://api.sandbox.paypal.com/2.0/)
* Sandbox API Signature SOAP [https://api-3t.sandbox.paypal.com/2.0](https://api-3t.sandbox.paypal.com/2.0)

**Paypal重定向地址**

* Live
  https://www.Paypal.com/cgi-bin/webscr?cmd=_express-checkout&amp;token=
* SandBox
  https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&amp;token=value

### 2.网址

* 开发者中心 [https://developer.paypal.com/](https://developer.paypal.com/)
* 各种源码下载 [https://www.paypal.com/IntegrationCenter/ic_downloads.html](https://www.paypal.com/IntegrationCenter/ic_downloads.html)
* API Error [https://www.paypal.com/IntegrationCenter/ic_api-errors.html](https://www.paypal.com/IntegrationCenter/ic_api-errors.html)

### 3.测试帐号

* Developer主帐号
[https://developer.paypal.com/](https://developer.paypal.com/)
* Seller
* Buye
* API验证信息
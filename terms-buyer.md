---
layout: page
title: Terms (buyer)
permalink: /terms-buyer/
---
These terms of service describe the usage of Paspagon (and its subdomains) from the buyer’s perspective. The service allows you to make cryptocurrency payments related to files stored in the cloud.

# Mechanism

After you get to an URL related to a particular file, an unique cryptocurrency payment address is generated and you a cookie with that information is set. (Multiple payment addresses may be generated if the seller supports multiple cryptocurrencies). This payment address is then combined with the required payment amount into a payment URI. If you make that payment (or a payment to the same address, but with higher amount) before a specified expiration time, and the payment is broadcasted and recorded by our servers, you may reload the page and you will be redirected to a page hosting the related file, with information about the payment you made included in the query string. The query string will contain a signature which, as a side effect, may allow you to download the file.

If for some reason your payment is not processed correctly, we may disclose a private key allowing you to redeem your funds. We however shall do that only in a finite time window starting some time after payment expiration time.

# Technical requirements

You should make the payment in a single transaction, with a transaction fee allowing the transaction to be included in a few blocks. You may not perform double spends on your transaction inputs. Your transaction inputs must have maximum sequence number set. If you violate these rules, your payment may not be accepted (this doesn’t exclude legal responsibility for performing double spends).

# Relation to the seller

Paspagon doesn’t provide you anything besides payment processing. We don’t guarantee that the file will still be present after the payment, that the hosting server will be up and running, that the file content will reflect advertisements etc. General terms of service which is related to the payment are constituted between you and the seller.

# Legal notes

This service is in the beta phase and is provided as is, without any implied or explicit warranties. You use it at your own risk. We disclaim liabilities to any losses related to using the service.

You cannot use the service if you are a customer from the New York state. Additionally, you cannot use the service from any location in which using or providing the service would constitute an illegal activity.

Payments on test networks are processed only for test purposes and do not constitute real transactions.

The service is provided by:
Myto sp. z o. o. sp. k.
Modrzewiowa 23
64-930 Dolaszewo
Poland
NIP: 7642670817
seed capital: 5000 zł
KRS: 0000587724 (Sąd Rejonowy Poznań-Nowe Miasto i Wilda Wydział Ⅸ Gospodarczy)

All disputes will be settled by the court applicable to the location of service provider.

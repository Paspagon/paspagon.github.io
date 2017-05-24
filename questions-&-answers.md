---
layout: page
title: Questions & answers
permalink: /questions-and-answers/
---
* TOC
{:toc}

### What is BlackCoin and why do you use it?

[BlackCoin](http://blackcoin.co/) is a cryptocurrency derived from Bitcoin which uses a different method to secure the network. This method leads to lower energy consumption, which in particular means lower transaction fees – a feature desired when processing micropayments. Another factor contributing to lower transaction fees is that BlackCoin network is able to process higher transaction volume than Bitcoin’s.

### Can I receive payments in fiat, not cryptocurrencies?

No. You can exchange cryptocurrencies you receive into fiat. This process can even be automated, so that end effect will we similar to receiving direct fiat payments. Paspagon doesn’t provide such conversion service, but nothing prevents you from pointing your payment address to one.

### Is it guaranteed that I will receive cryptocurrencies for each accepted payment?

No. Payments in general are processed shortly after they are made. The specifics of cryptocurrencies is that you need to wait some time before you may feel assured that the payment you received is “confirmed”. This waiting time is inconvenient when you sell files online and it may be abandoned, but it means that fraudulent user may have some chances of performing a transaction which will emerge as uncorfirmed. We do take some precautions to detect such behaviours, but no guarantees can be made. In particular, there is [an attack described which may allow an advanced BlackCoin user to cheat with a high probability of success](https://www.reddit.com/r/blackcoin/comments/3xz8za/bounty_for_porting_bitcoinxt_patch_into_blackcoin/cyz2rlr). This attack is however limited in its possible frequency.

### How will you notify me that a payment has been processed?

Your customer will be redirected to the S3 page related to the file for which he made the payment. The redirection URL will contain informations about the payment and an authorization signature (valid for a specific period of time) which, as a side effect, should allow him to download the file. You will probably want to enable [bucket logging](http://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html) to obtain information about successful payments.

### Will the customer be able to download the file only once?

No. The authorization signature that he receives allows him to start downloading the file multiple times for a specific period of time. If you host large files, you may want to lower that time to mitigate risks related to downloading files multiple times (and being charged for S3 bandwidth). We dissuade selling large files to anonymous customers.

### Who pays for file storage and file transfer bandwidth?

You handle file storage and you pay for it. Note that Amazon provides a [free tier programme](https://aws.amazon.com/free/) which allows you to use some services for free to a limited degree during the introductory period.

### Will a file be downloaded or displayed in the browser?

It depends on the file type and the customer browser’s capabilities and configuration. A common behaviour is to display the file if the browser is capable of doing that. You can try to control that using the [`content-disposition` header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec19.html#sec19.5.1).

Be careful with displaying files in the browser. S3 URLs have expiration time set, so if an user after viewing a file tries to save it to disc, he might not be able to do it. If your main intention is that an user should save the file, you may want to set the `content-disposition` header accordingly.

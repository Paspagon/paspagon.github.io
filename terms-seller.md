---
layout: page
title: Terms & manual (seller)
permalink: /terms-seller/
---

These terms of service describe the usage of Paspagon (and its subdomains) from the seller’s perspective. The service allows you to process cryptocurrency transactions constituting payments for files stored by you in cloud storage systems. The following storage providers are supported:

* [Amazon S3](https://aws.amazon.com/s3/) (selected regions only)

The following cryptocurrencies are supported:

* [Bitcoin](https://bitcoin.org)
* [BlackCoin](http://blackcoin.co)

## Configuration

You can start using Paspagon in two steps:

### *Step 0A. Get accustomed to cryptocurrencies*

Since you’re going to get paid in cryptocurrencies, you should know how to use them. This is a deep topic which we don’t aim to cover here – we recommend exploring Bitcoin’s and BlackCoin’s websites to gain some knowledge and choose wallets for receiving payments.

### *Step 0B. Get accustomed to cloud storage and create a bucket*

This is again a deep topic which we don’t aim to cover; we recommend reading the [Getting Started with Amazon S3 guide](https://aws.amazon.com/s3/getting-started/). After that, you should create at least one bucket in a region supported by Paspagon (see below for a list of supported regions) which will act as a container to your files.

### Step 1. Grant us read access to your bucket

To process payments for your files, we need to have read access to them. We will not ever read contents of the files you sell, only metadata will be retrieved. You need to give read permission to the `154072225287` AWS User.

#### *Bucket access grant instruction*

To grant us read access to your bucket, you may select your bucket in AWS Management Console, and then click “Properties” ➜ “Permissions” ➜ “Add bucket policy”. A text field will emerge which you can fill using the following template:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Resource": "arn:aws:s3:::𝗬𝗢𝗨𝗥-𝗕𝗨𝗖𝗞𝗘𝗧-𝗡𝗔𝗠𝗘/*",
    "Sid": "PaspagonAllow",
    "Effect": "Allow",
    "Principal": {"AWS": "154072225287"},
    "Action": ["s3:GetObject"]
  }
}
```

### Step 2. Configure payments for your bucket

You need to upload a special file named `paspagon.ini` to your bucket. This should be a file in the [INI format](https://en.wikipedia.org/wiki/INI_file) which adheres to the following template:

```ini
accept-terms = 𝗧𝗘𝗥𝗠𝗦-𝗨𝗥𝗟
authorization-keys-der-base64 = 𝗢𝗣𝗧𝗜𝗢𝗡𝗔𝗟-𝗞𝗘𝗬𝗦
country-𝗥𝗘𝗦𝗧𝗥𝗜𝗖𝗧𝗜𝗢𝗡-𝗧𝗬𝗣𝗘 = 𝗧𝗪𝗢-𝗟𝗘𝗧𝗧𝗘𝗥-𝗖𝗢𝗗𝗘-𝗟𝗜𝗦𝗧

[payment]
price-𝗖𝗨𝗥𝗥𝗘𝗡𝗖𝗬-𝟏 = 𝗣𝗥𝗜𝗖𝗘
address-𝗖𝗨𝗥𝗥𝗘𝗡𝗖𝗬-𝟐 = 𝗔𝗗𝗗𝗥𝗘𝗦𝗦
link-expiration-time = 𝗢𝗣𝗧𝗜𝗢𝗡𝗔𝗟-𝗧𝗜𝗠𝗘-𝗜𝗡-𝗦𝗘𝗖𝗢𝗡𝗗𝗦

[seller]
email = 𝗬𝗢𝗨𝗥-𝗘𝗠𝗔𝗜𝗟
country-code = 𝗧𝗪𝗢-𝗟𝗘𝗧𝗧𝗘𝗥-𝗖𝗢𝗗𝗘
```

Below is an example config file with explanations.

```ini
# You need to specify URL of the terms of service that you accept. Any other
# value than the below one will result in inability to generate payments.
accept-terms = https://github.com/Paspagon/paspagon.github.io/blob/master/terms-seller.md
# If you want to explicitly authorize payment generation, you specify public
# authorization keys here. If you don’t want this feature, you can delete the
# line below or leave the field empty.
authorization-keys-der-base64 =
# Optional geographical restriction for payment generation. You can specify
# either country-whitelist or country-blacklist.
country-code-whitelist = US,CN,RU,BR

[payment]
# Below you specify default values for your payments. They may be overridden
# for each file by setting file metadata.

# Price expressed in the currency you choose.
# The currency must be indicated using the three-letter, uppercase symbol.
price-XAU = 0.001
# Payment dispatch addresses for selected cryptocurrencies.
# If you don’t specify an address for a cryptocurrency, payments for it won’t
# be generated.
# The symbols are: BTC for Bitcoin and BLK for BlackCoin.
address-BTC = 1BTCinvalid
address-BLK = Blkinvalid
# Expiration time for the URLs to which your customers are redirected after a
# successful payment (in seconds). Default is 60.
link-expiration-time = 15

[seller]
# Email is not mandatory, but strongly recommended. We reserve the right to use
# it to contact you on subjects related to the service.
email = your_email@example.com
# Country code is required.
country-code = HU
```

The config file should be encoded in UTF-8. Only the first 512 bytes of the file will be read.

#### Currencies supported for specifying prices

- BTC — Bitcoin
- BLK — BlackCoin
- XAU — troy ounce of gold
- USD — US dollar
- EUR — euro
- PLN — Polish zloty
- HUF — forint
- CHF — Swiss franc
- CZK — Czech koruna
- GBP — pound sterling

Some other currencies may also work, but we don’t officially support them.

We disclaim any liability to results of inacurrate number rounding or making conversions based on outdated exchange rates.

#### Minimum prices

The prices you specify should evaluate to at least 0.001 BTC or 1 BLK, respectively. If you specify lower prices, payments may be not generated.

#### Per-file payment settings

You may override settings from the `[payment]` section for each individual file. To do this, you need to set [object metadata](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html). For example, to override price to $2.3456, you need to set the `x-amz-meta-price-USD` header to `2.3456`.

#### Caching

We may cache payment configuration for no more than 3 hours.

## Payment generation

After you make necessary configuration, you can point your customers to URLs in the format `https://𝗣𝗔𝗦𝗣𝗔𝗚𝗢𝗡-𝗥𝗘𝗚𝗜𝗢𝗡.paspagon.com/𝗕𝗨𝗖𝗞𝗘𝗧-𝗡𝗔𝗠𝗘/𝗙𝗜𝗟𝗘-𝗡𝗔𝗠𝗘`. File names may not be longer than 256 bytes.

### Country restriction for payment generation (optional)

You can authorize payment generation based on IP-determined country. We use the [MaxMind’s](http://www.maxmind.com) GeoLite Country IPv6 database (we don’t make any guarantees about how often we update that database). *You may want to look at the [ISO 3166 country codes list](http://dev.maxmind.com/geoip/legacy/codes/iso3166/) used by GeoIP databases.* A special code `UNKNOWN` is used when the country cannot be determined.

### Payment generation authorization (optional, for advanced users)

You can grant explicit authorization for each payment generation, discarding the simple method described above. This also allows you to associate custom buyer data with each payment. The relevant steps with reference GNU/Linux commands are provided below.

To be able to authorize a customer request, you have to have a private ECDSA secp256k1 key. The following command allows you to generate one (assuming you have OpenSSL 1.0.2 installed):

```bash
openssl ecparam -out paspagon-authorization.priv -name secp256k1 -genkey
```

Next, compute a public key:

```bash
openssl ec -in paspagon-authorization.priv -pubout -outform DER -conv_form compressed | base64 -w 0
```

The result of this command is a compressed, DER-formatted and base64-encoded public key. Paste it as the value of the `authorization-keys-der-base64` field in the config file. If you want to use multiple keys, you can separate them by comma.

To create an authorized URL for a particular file, you construct a canonical URL by concatenating its standard URL and the following query string parameters, byte-ordered and then [URL encoded](https://en.wikipedia.org/wiki/Percent-encoding):

- `expires`: [Unix time](https://en.wikipedia.org/wiki/Unix_time) after which the authorization will expire;
- `signature-algorithm`: `P1-SHA256`;
- optionally, any custom buyer data associated to the payment, with the `buyer-` key prefix. Custom buyer data may not be longer than 256 bytes when URL-encoded.

For example, to create an authorization for an object named `foo.txt`, stored in a bucket named `samplebucket` in the `s3-us-west-1` region, with the buyer name set to “Grzegorz Brzęczyszczykiewicz” and buyer location set to “Chrząszczyrzewoszyce, powiat Łękołody”, expiring at 15th of July 2010 at 10 AM UTC, you construct the following URL:

```
https://s3-us-west-1.paspagon.com/sample-bucket/foo.txt?buyer-location=Chrz%C4%85szczyrzewoszyce%2C+powiat+%C5%81%C4%99ko%C5%82ody&buyer-name=Grzegorz+Brz%C4%99czyszczykiewicz&expiration=1279188000&signature-algorithm=P1-SHA256
```

The URL must be signed using the private key and the `SHA256` digest, applying the base64 encoding.

```bash
echo -n "𝗖𝗔𝗡𝗢𝗡𝗜𝗖𝗔𝗟-𝗨𝗥𝗟" | openssl dgst -sha256 -sign secp256k1.priv  | base64 -w 0
```

The result needs to be added to the query string, URL-encoded under the `signature` parameter (ordering of the query string parameters does not matter).

The final URL will look like the following:

```
https://s3-us-west-1.paspagon.com/sample-bucket/foo.txt?buyer-location=Chrz%C4%85szczyrzewoszyce%2C+powiat+%C5%81%C4%99ko%C5%82ody&buyer-name=Grzegorz+Brz%C4%99czyszczykiewicz&expiration=1279188000&signature-algorithm=P1-SHA256&signature=MEUCIQDMSUfo3JDkfqatj66vw4viDmYWh166MNICvgA24KJTgQIgcrkAI25dSN0cRSekK0Non8cqucGVRoj2%2Bfg96TaoKXU%3D
```

## Regions provided

Paspagon region | S3 region | Location | Supported currencies
--------------- | --------- | -------- | --------------------
[s3-us-west-2](https://s3-us-west-2.paspagon.com) |  `us-west-2` | Oregon | Bitcoin, BlackCoin
[s3-eu-west-1-test](https://s3-eu-west-1-test.paspagon.com) |  `eu-west-1` | Ireland | Bitcoin Testnet

## Payment dispatches

Payments are processed according to the [buyer’s terms of service](/terms-buyer), which constitute a separate document, being however an integral part of this one. After the download link is send to the buyer and transactions related to his payments get enough confirmations, those transactions *(typically one)* are dispatched to the address you specified.

You acknowledge that the generated download link may be used by the buyer multiple times before expiration and that, since the service is a zero-confirmation system, some transactions may not get their way into a block and therefore not be dispatched to you despite download link being sent. *This generally should not happen in the case of a honest buyer and we take some precautions against fraudulent ones, but we cannot guarantee that they will be 100% effective.*

Paspagon may not dispatch transaction inputs which are less than minimum payments.

## Fees

For each payment, Paspagon charges fees per each transaction used to dispatch this payment to the address you specified. The fee consists of:

- constant part, which cannot be higher than 0.0004 BTC or 0.4 BLK
- variable part, which cannot be higher than 5% of the sum of transaction inputs

*The actual transaction fees may vary per each region and are published on the region pages.*

## Legal notes

This service is in the beta phase and is provided as is, without any implied or explicit warranties. You use it at your own risk. We disclaim liabilities to any losses related to using the service.

Sections and text fragments marked with italics are non-normative and provided only for educational purposes. The same applies to hyperlinks unless their content is referenced by a normative fragment.

We can change these terms at any time by pushing an update to [the website’s repository](https://github.com/Paspagon/paspagon.github.io/). *You can track the changes using [an Atom feed](https://github.com/Paspagon/paspagon.github.io/commits/master/terms-seller.md.atom).*

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

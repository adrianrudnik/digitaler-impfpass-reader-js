# QR-Code payload decoder of the digital vaccination passport as issued by the European eHealth network

This is a simple study on how to validate the QR codes flying
around in peoples pockets with a client side browser instance for
various reasons.

JavaScript is not my daily driver, so do not expect super shiny code.

I used this repo to research a possible integration of the german
Digitaler Impfpass to an already existing QR code reader based
entry control solution.

I aborted the attempt as the pass contains more information than needed,
like full personal details AND medical information about what happened
with the mentioned person. This should not be part of consumer apps,
distributed on consumer ways without any audit log or even access log,
but that's just my personal opinion. Just forcing stuff offline should
not free people from the responsibility to care about the issued
information, and it's possible malicious collection or processing.

## Project structure

`lib.mjs` contains the decoding, reading and parsing parts.

`certificates.mjs` contains a static dump of the current issued and
allowed signers. See the comments on how and where to get them.

`example.mjs` is my test on my valid certificate.

## Covert QR to string on shell

I save the screenshot of the output from CovPass or Corona Warn App into
a JPG and do this on linux:

```bash
sudo apt-get install zbar-tools
zbarimg "code.jpg"
```

to extract a valid payload from it. Put the result into the `qrCode`
variable you can find in `example.mjs` for a test run.

## Run locally with node

Just put a valid LIVE QR code content into the `qrCode` variable inside
`example.mjs`.

This does not work with staging certificates.

```bash
yarn install
node example.mjs
```

Output will be something like this:

```js
{
  certificates: [
    {
      CertificateIdentifier: 'URN:UVCI:01DE/A68...#1',
      CountryOfVaccination: 'DE',
      DoseNumber: 1,
      DateOfVaccination: '2021-03-10',
      CertificateIssuer: 'Robert Koch-Institut',
      MarketingAuthorizationHolder: 'ORG-100030215',
      VaccineMedicinalProduct: 'EU/1/20/1528',
      TotalSeriesOfDoses: 2
    }
  ],
  person: {
    Birthday: '1960-04-03',
    Surname: 'x',
    SurnameStandardised: 'X',
    Forename: 'Y',
    ForenameStandardised: 'Y'
  }
}
```

This is just an example, and it does not read all available information
from it. Just parts to illustrate on how to.

## Why not build a clean lib?

I'm not into NPM. JavaScript is not my primary language. I do not have
the time to maintain this, I need to pay my rent and this is not a topic
I could  earn a living from. Feel free to fork and improve.

## Why Apache licensed?

I'm not a lawyer. Most, if not all, public code around this eHealth stuff
is in Apache licensed, so I stuck with it. Want to change the license?
Feel free to do so on your own, I do not care for this example repo.
Other entities might though.

## Additional resources

To get a full overview of the available information on the payload, see

https://github.com/ehn-dcc-development/ehn-dcc-schema/blob/release/1.3.0/DCC.Core.Types.schema.json

https://github.com/ehn-dcc-development/ehn-dcc-schema/blob/release/1.3.0/DCC.Types.schema.json

Also, the example repository was pretty helpful:

https://github.com/ehn-dcc-development/dcc-testdata

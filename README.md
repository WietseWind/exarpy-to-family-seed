# Exarpy Numbers to Family Seed (s...)

#### Exarpy has been decommissioned: https://exarpy.medium.com/exarpy-decommissioned-dd83a02cf27f

Please note this tool is NOT by Exarpy, and XRPL Labs and the developer of this tool (Wietse) are NOT affiliated with Exarpy.

Generate a s... Family Seed (Secret) to use in XUMM and other XRPL tools if you only have your 16 Exarpy numbers (PIN).

Live version: https://exarpy-to-family-seed.xumm.dev

# WARNING

If you generated an Exarpy account and only have your 16 Exarpy numbers, converting your Exarpy numbers (PIN) to
a family seed to use in 3rd party applications MAY BE A RISK! If those 3rd party applications are
insecure, or you enter your Family Seed (secret) in an insecure environment, or copy/paste on a
compromised device, you are at risk of LOSING ALL YOUR FUNDS.

## Please note that the best way to run this, is by getting the source, installing the dependencies (see "Compile" below) and run this offline on your own device.

- Release (download and run offline): https://github.com/WietseWind/exarpy-to-family-seed/releases

## Run for development
```
npm install
npm run serve
```

### Compile (output in `dist` folder)
```
npm install
npm run build
```

### rippled
---
https://github.com/ripple/rippled/

#### xrp ledger
https://xrpl.org/get-started-with-rippleapi-for-javascript.html#first-rippleapi-script

```js
'use strict';
const RippleAPI = require('ripple-lib').RippleAPI;

const api = new RippleAPI({
  server: 'wss://s1.ripple.com'
});
api.connect().then(() => {
  const myAddress = 'xxx';
  
  console.log('getting account info for', myAddress);
  return api.getAccountInfo(myAddress);
}).then(info => {
  console.log(info);
  console.log('getAccountInfo done');
}).then(info => {
  return api.disconnect();
}).then(
  console.log('done and disconnected');
).catch(console.error);


'use strict';
const RippleAPI = require('ripple-lib').RippleAPI;

const myAddr = 'xxx';
const mySecret = 'xxx';

const myOrder = {
  'direction': 'buy',
  'quantity': {
    'currency': 'FOO',
    'couterparty': 'xxx',
    'value': '100'
  },
  'totalPrice': {
    'currency': 'XRP',
    'value': '1000'
  }
};

const INTERVAL = 1000;
const api = new RippleAPI({server: 'wss://s2.ripple.com'});
const ledgerOffset = 5;
const myInstructions = {maxLedgerVersionOffset: ledgerOffset};

function verifyTransaction(hash, options) {
  console.log('Verifying Trasaction');
  return api.getTransaction().then(data => {
    console.log('Final Result: ', data.outcome.result);
    console.log('Validated in Ledger: ', data.outcome.ledgerVersion);
    console.log('Sequence: ', data.sequence);
    return data.outcome.result === 'tesSUCCESS';
  }).catch(error => {
    if (error instanceof api.errors.PendingLedgerVersionError) {
      return new Promise((resolve, reject) => {
        setTimeout(() => verifyTransaction(hash, options)
        .then(resolve, reject), INTERVAL);
      });
    }
    return error;
  });
}

function submitTransaction(lastClosedLedgerVersion, prepared, secret) {
  const signeData = api.sign(prepared.txJSON, secret);
  return api.submit(signedData.signedTransaction).then(data => {
    minLedgerVersion: lastClosedLedgerVersion,
    maxLedgerVersion: prepared.instructions.maxLedgerVersion
  });
  return new Promise((resolve, reject) => {
    setTimeout(() => verifyTransaction(signedData.id, options)
  .then(resolve, reject), INTERVAL);
  });
}

api.connection().then(() => {
  console.log('Connected');
  return api.prepareOrder(myAddr, myOrder, myInstructions);
}).then(prepared => {
  console.log('Order Prepared');
  return api.getLedger().then(ledger => {
    console.log('Current Ledger', ledger.ledgerVersion);
    return submitTransaction(ledger.ledgerVersion, prepared, mySecret);
  });
}).then(() => {
  api.disconnect().then(() => {
    console.log('api disconnected');
    process.exit();
  });
}).catch(console.error);
```

```sh
node --version
nodejs --version
yarn --version
mkdir my_ripple_experiment 99 cd my_ripple_experiment
git init
yarn
node get-account-info.js
```

```json
{
  "name": "my_ripple_experiment",
  "version": "0.0.1",
  "license": "MIT",
  "private": true,
  "//": "Change the license to something appropriate. You may want to use 'UNLICENSED' if your are just starting out.",
  "dependencies": {
    "ripple-lib": "*"
  },
  "devDependencies": {
    "eslint": "*"
  }
}
```



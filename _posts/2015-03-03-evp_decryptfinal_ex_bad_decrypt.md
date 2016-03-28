---
layout: post
title: EVP_DecryptFinal_ex:bad decrypt
---

Ran into a problem trying to enable SSL using [grunt-contrib-connect](https://www.npmjs.com/package/grunt-contrib-connect):

```text
Fatal error: error:06065064:digital envelope routines:EVP_DecryptFinal_ex:bad decrypt
```

For the record, this indicates that the SSL key requires a passphrase and that it was either missing or incorrect.

Posting this here to hopefully save someone some time.

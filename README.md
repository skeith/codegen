![128x128](https://user-images.githubusercontent.com/2068077/27226478-1f04d1d0-5254-11e7-95b7-00ebe45def79.png)

# Code Generator
Google Forms add-on that generates codes upon form submission. This add-on was borne of my need to establish a quiz as a *pre-requisite* to filling out a second form.

- Optionally, require a passing quiz score (if you've set it up as a quiz)
- Set a custom salt.
- Optionally, email the code to yourself as well. (only if the student passes the quiz, or if no passing score is required)

Now available through the [Google Forms Web Store](https://chrome.google.com/webstore/detail/form-code-generator/dobboooonnlomcgidfnpmeipdcdennnh). Need to verify these codes? See the section `#Verification` below. A Google Spreads add-on is coming soon.

> Form Code Generator was created for lightweight use cases only. It should not be used for complex or important workflows, as the security of these hash codes are not generated per state-of-the-art standards (e.g., does not support nonce values).

Disclaimer: I used plenty of code from Google Form Add-on's [original tutorial](https://developers.google.com/apps-script/quickstart/forms-add-on). In several cases, I even left Google's original copyright notice.

![screen shot 2017-06-16 at 4 59 00 am](https://user-images.githubusercontent.com/2068077/27226486-27bb1f64-5254-11e7-88cc-72a307145535.png)

![screen shot 2017-06-22 at 2 59 10 pm](https://user-images.githubusercontent.com/2068077/27457941-6941911e-575b-11e7-9ce0-b7ccae058229.png)

# Verification

In effect, I use an `MD5` hash combining the user's email address, a salt the user sets, and a fixed random string to prevent rainbow tables.

1. Check the salt used in your form code generator. (Go to “Form Code Generator” > “Configurations”. In the side menu, the first form input is your salt.)
2. In the relevant Google spread, click on “Tools” > “Script Editor”, and paste the following code below. Save the script. (`ctrl+s` or `cmd+s`)

````
function MD5 (input) {
  var rawHash = Utilities.computeDigest(Utilities.DigestAlgorithm.MD5, input);
  var txtHash = '';
  for (i = 0; i < rawHash.length; i++) {
    var hashVal = rawHash[i];
    if (hashVal < 0) {
      hashVal += 256;
    }
    if (hashVal.toString(16).length == 1) {
      txtHash += '0';
    }
    txtHash += hashVal.toString(16);
  }
  return txtHash;
}
````

3. In the relevant Google spread, add the following to F2 to see the hash code for row 2’s submission:

```
=MD5(C2+YOUR_SALT + "dEstr0yR@1nB0wTAb1es”)
```

Make sure to replace `YOUR_SALT`.
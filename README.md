# BioEncryption: Biometric Encryption 

# 1. What Problem Does BioEncryption Solve?

Suppose you have a secret on your mobile device you want to hide. How
would you hide it? You have several options:

1.  *Use a Lock/Encrypt feature on your phone*. The iPhone for example,
    has a Lock feature which allows lock and unlock operations using the
    phone’s biometric authentication system. However, actual lock/unlock
    is performed using a password, which the iPhone stores somewhere on
    the device; biometric authentication only grants seamless retrieval of that
    password.

2.  *Store the secret using some password-protection App*. Clearly, the
    secret exists inside the App, and anyone who know the App’s password
    can reveal the secret; that password is stored on the phone.

The problem with such solutions is that your secret depends on some other secret which is stored
on the mobile-device and poses a security risk.

# 2. The BioEncryption Solution

With BioEncryption, your secret is not stored at all. Rather, it is
used to modify Machine Learning biometric classification models so that
it can only be revealed using *<u>your biometrics</u>*. 

Note that BioEncryption is *not* about  *generating* Biometric features; they can be generated from facial images (e.g., using openCV), fingerprints, iris features, etc. Rather, BioEncryption *is* about secret hiding using given feature files.

# 3. How Does BioEncryption Work?

BioEncryption contains wo functions, *Hide* and *Reveal*.

1.  *Hide*. Hide’s inputs are your secret, and train and test (binary)
    labeled feature-files for creating Machine Learning classifier based
    on your biometrics. It generates a model file with the biometric
    authentication classifier that *represents* your secret.

2.  *Reveal*. Reveal’s inputs are the model file generated by Hide, and
    a file with one row/vector of “runtime” biometric features. It will
    reveal the correct secret only if the runtime feature-vector
    contains your features and will generate gibberish otherwise (we have no way of comparing the outcome to the true secret, because that secret is no longer stored anywhere).

Clearly, you should remove the train and test feature-files from your
device once *Hide* is complete, otherwise an attacker can use a feature
vector taken from those files as input to Reveal, thereby revealing your
secret.

# 4. Other Applications

BioEncryption is not restricted to biometric classification features. BioEncryption can
hide a secret using any set of feature vectors. For example, the runtime
stack trace of an App *A* can be used to hide a secret so that only *A*
will be able to reveal it.

As suggested in the literature, BioEncryption can also be used to hide and reveal a secret key so that no one but the legitimate user can perform digital signatures.

# 5. Using Hide and Reveal

Hide and Reveal are two API functions inside BioEncryption.jar. They are invoked
from the command-line.

## 5.1 Hide

Hide requires two inputs: the secret, and a properties file (discussed
below). Two examples are of executing Hide:

1.  java -cp BioEncryption.jar Hide "Lorem ipsum dolor sit amet, consectetur
    ..."

"Lorem ipsum dolor sit amet, consectetur ..." is the secret; it should
be a string wrapped in quotes. In this example the properties file is
not explicitly specified in the command-line; it therefore defaults to
*properties.txt* that resides as a sibling file to BioEncryption.jar.

2.  java -cp BioEncryption.jar Hide "Lorem ipsum dolor sit amet, consectetur
    ..." /Users/jeffprat/Documents/temp/properties.txt

"Lorem ipsum dolor sit amet, consectetur ..." is the secret; it should
be a string wrapped in quotes. In this example the properties file is
explicitly specified in the command-line using an absolute path.

### The properties File for Hide

> the properties.txt file used by Hide is a kay-value pairs file, and
> must contain the following pairs; obviously, the values are just
> example values:

-   numFeatures = 500 \#number of features; each train-file row and and
    test-file row must have this number of (comma-separated) features,
    followed by a (comma-separated) "Y=n" at the end, with n being 1 or 0

-   Train_M = 1000 \#number of training rows, each row is
    comma-separated list of "numFeatures" features

-   Test_M = 1000 \#number of testing rows, each row is comma-separated
    list of "numFeatures" features

-   TrainFile = sample_data/sample_train.csv \# Relative path of train CSV
    file. It should be sibling to this this properties file

-   TestFile = sample_data/sample_test.csv \# Relative path of test CSV
    file. It should be sibling to this properties file

-   serializedModelFile = JohnDoeModel.be \# Name of file used for
    serializing model

## 5.2 Reveal

Two examples are of executing Reveal:

1.  java -cp BioEncryption.jar Reveal JohnDoeModel.be sample_data/sample_runtime.csv

*JohnDoeModel.be* is the relative path model file that Hide generated
(see section 5.5.1), and *sample_data/runtime_0.txt* is the (relative
path) runtime feature-vector file – its format is discussed in section
5.3.

2.  java -cp BioEncryption.jar Reveal /Users/jeffprat/Documents/JohnDoeModel.be
    /Users/jeffprat/Documents/sample_data/sample_runtime.csv

This example uses absolute file paths instead of relative file paths.

## 5.3 Structure of train, test, and runtime feature-vector files 

All files a comma-separated (csv) files. The train and test CSV file paths are
specified in properties.txt for use by the Hide operation. The runtime
feature-vector CSV file is specified as a command-line argument to Reveal
(see section 5.2). Samples of all three types of files are provided in the *sample_data* folder.

-   The train file (e.g., sample_data/sample_train.txt specified in
    section 5.1.1) should contain *Train_M* rows of *numFeatures*
    features, each followed by a 1 or 0  classification label.

-   The test file (e.g., sample_data/sample_test.txt specified in
    section 5.1.1) should contain *Test_M* rows of *numFeatures*
    features, each followed by a 1 or 0  classification label.

-   The runtime runtime feature-vector CSV file should contain one row of
    *numFeatures* features. Obviously, being runtime classification
    feature-vector, it contains no label at the end of the row.

# 6. Licensing

The version on github is an *evaluation version*. Reveal will work only
within one week of performing Hide. Besides evaluating the product you
cannot distribute or otherwise use it.

Contact us to inquire about licensing the full version of any other
serious business offer, at *jeffprat at protonmail.com*


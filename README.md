python3-pywbem
==============

This is a fork of PyWBEM for Python 2.6+ and Python 3.4+

Motivation
----------
When working with WBEM/CIM and Python, PyWBEM is the only library available. Even libraries like PowerCIM use PyWBEM under the hood. Unfortunately, PyWBEM only supports the Python 2.x series and carries a heavy dependency, M2Crypto, that is difficult to install on platforms other than Linux. Worse yet, this dependency is also restricted to the Python 2.x series. There have been some efforts to update M2Crypto for Python 3, but none seem to be in a fully working state.

This fork introduces Python 3.4+ support by dropping M2Crypto in favor of Python's newly updated, built-in `ssl` module. This module improves Python's SSL security enough that M2Crypto is no longer required. However, Python 2.6+ still requires M2Crypto. Compatibility with the 2.x series should make the transition to Python 3.4+ seamless.

Dependencies
------------
With Python 2.6+:
* six
* M2Crypto

With Python 3.4+:
 * six
 
For Python 3.4+, the entire PyWBEM stack is pure-Python!

Installation
------------
There is no PyPI package or anything yet. I've never created one before, but if you'd like one, feel free to contribute! For now, make a `pywbem` folder in your site-packages folder and put the contents of this repo inside it.

Documentation
-------------
See the official documentation: http://pywbem.sf.net/

Examples
--------
The following is copied from the official examples (see documentation) for convenience:

```python
import pywbem

# Make connection
conn = pywbem.WBEMConnection('https://server',     # url
                             ('root', 'penguin'))  # credentials

# Enumerate CIM_Process instance names and instances

instance_names = conn.EnumerateInstanceNames('CIM_Process')
instances = conn.EnumerateInstance('CIM_Process')

print '%d CIM_Process names returned' % len(instance_names)
print '%d CIM_Process instances returned' % len(instance_names)

# Get all CIM_OperatingSystem instances

names = conn.EnumerateInstanceNames('CIM_OperatingSystem')

# Call GetInstance on returned instances

for n in names:
    os = conn.GetInstance(n)
    for key, value in os.items():
        print '%s = %s' % (key, value)
```

Frequently Asked Questions
--------------------------
Q: Why fork PyWBEM?

A: See the Motivation section.


Q: Why do I still need M2Crypto for Python 2.6+?

A: Unfortunately, the `ssl` module in Python 2.6+ doesn't have the SSL certificate options and checks it should have. Recently, Python 2.7.9 was released with some backported `ssl` code that brings it up to parity (nearly) with Python 3.4. However, since M2Crypto is available for the 2.x series in general, it didn't make much sense to port that specific release to using the built-in `ssl` module.


Q: Why isn't Python 3.3 (or 3.2) supported?

A: Python's `ssl` module got an update for 3.4 that made it more secure. Specifically, adding the `create_default_context` function which sets some sane defaults depending on the environment.


Q: How difficult was it to port PyWBEM to Python 3 while maintaining Python 2.6+ compatibility?

A: Short answer: more difficult than I'd hoped. From what I can tell, PyWBEM supports Python 2.3+ (maybe even earlier releases). That meant a lot of the code was calling deprecated methods even by Python 2.6 standards. I'm not going to lie, the conversion of `NocaseDict` was the cause of many problems during testing and I can't tell you how many `has_key` calls needed to be converted to the more modern `in` syntax.


Future Enhancements
-------------------
* If I have time, I will try to keep up with the official PyWBEM repo on SourceForge and make updates when needed
* PyWBEM's implementation is kind of a mess and not very friendly to use. If I have the time/motivation, I may try to create a higher-level helper class to make things easier to use.

Contributions
-------------
Did you see something in the FAQ or the Future Enhancements sections that you want to help with? Did you find a bug? Do you want to help this fork become even more powerful than the original?

Excellent! Feel free to add your contributions through the standard means here on GitHub. I've done some Python 3 conversions for different modules before, but this was by far the most difficult of them so far. I'd love some help improving this module, cleaning up some old code, adding features, documentation, tests, or whatever you think could help.

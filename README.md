Hexahexacontadecimal
====================

**Hexahexacontadecimal is the most compact way to encode a number into a URL.**

Hexahexacontadecimal is a compact format to express a number in a URL. It uses all characters allowed in
a URL without escaping -- the [unreserved characters](http://tools.ietf.org/html/rfc3986#section-2.3) --
making it the shortest possible way to express an integer in a URL.

## Usage

    from hexahexacontadecimal import hexahexacontadecimal_encode_int, hexahexacontadecimal_decode_int

    print hexahexacontadecimal_encode_int(302231454903657293676544)  # 'iFsGUkO.0tsxw'
    print hexahexacontadecimal_decode_int('iFsGUkO.0tsxw')           # 302231454903657293676544L

Note that `urllib.quote` [escapes the tilde character (~)](http://bugs.python.org/issue16285), which is not necessary as
of RFC3986.

## On the command line

With [pyle](https://github.com/aljungberg/pyle) you can easily use hexahexacontadecimal on the command line.

    $ wc -c LICENSE MANIFEST setup.py | pyle -m hexahexacontadecimal -e "'%-10s Hexhexconta bytes:' % words[1], hexahexacontadecimal.hexahexacontadecimal_encode_int(int(words[0]))"
    LICENSE    Hexhexconta bytes: MV
    MANIFEST   Hexhexconta bytes: 1z
    setup.py   Hexhexconta bytes: GI
    total      Hexhexconta bytes: ei

### Hexahexacontadecimal vs Base 64 in URLs

    >>> n = 292231454903657293676544
    >>> import base64
    >>> urlquote(base64.urlsafe_b64encode(long_to_binary(n)))
    'PeHmHzZFTcAAAA%3D%3D'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    'gpE4Xoy7fw5AO'

Worst case scenario for plain Base 64:

    >>> n = 64 ** 5 + 1
    >>> urlquote(base64.urlsafe_b64encode(long_to_binary(n)))
    'QAAAAQ%3D%3D'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    'ucrDZ'

Worst case for hexahexacontadecimal:

    >>> n = 66 ** 5 + 1
    >>> urlquote(base64.urlsafe_b64encode(long_to_binary(n)))
    'SqUUIQ%3D%3D'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    '100001'

That big SHA-512 you always wanted to write in a URL:

    >>> n = 2 ** 512
    >>> urlquote(base64.urlsafe_b64encode(long_to_binary(n)))
    'AQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA%3D'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    'JK84xqGD9FMXPNubPghADlRhBUzlqRscC2h~8xmi99PvuQsUCIB2CHGhMUQR8FLm72.Hbbctkqi89xspay~y4'

Massive savings.

### Are the savings really significant?

If you're currently doing your BASE64 encoding the naive way, then yes:

    >>> sum(len(urlquote(base64.urlsafe_b64encode(long_to_binary(n)))) for n in xrange(10 ** 5))
    531584
    >>> sum(len(urlquote(hexahexacontadecimal_encode_int(n))) for n in xrange(10 ** 5))
    295578

### But what if I use Base64 without padding?

Then the savings are not as significant. But it's still an improvement. Using the code from [this StackOverFlow question](http://stackoverflow.com/a/561704/76900):

    >>> from hexahexacontadecimal.num_encode_base64 import num_encode as num_encode_base64
    >>> n = 64 ** 5 + 1
    >>> urlquote(num_encode_base64(n))
    'BAAAAB'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    'ucrDZ'
    >>> n = 66 ** 5 + 1
    >>> urlquote(num_encode_base64(n))
    'BKpRQh'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    '100001'
    >>> n = 2 ** 512
    >>> urlquote(num_encode_base64(n))
    'EAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA'
    >>> urlquote(hexahexacontadecimal_encode_int(n))
    'JK84xqGD9FMXPNubPghADlRhBUzlqRscC2h~8xmi99PvuQsUCIB2CHGhMUQR8FLm72.Hbbctkqi89xspay~y4'
    >>> sum(len(urlquote(num_encode_base64(n))) for n in xrange(10 ** 5))
    295840
    >>> sum(len(urlquote(hexahexacontadecimal_encode_int(n))) for n in xrange(10 ** 5))
    295578

Why settle for less than perfect?

## Installation

    pip install hexahexacontadecimal

## Documentation

This file and docstrings.

## License

Free to use and modify under the terms of the BSD open source license.

## Author

Alexander Ljungberg

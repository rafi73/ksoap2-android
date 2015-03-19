# Using Cookies in ksoap2-android #

Many web services use cookies to maintain state between different calls to the web service. ksoap2-android exposes enough of the underlying HTTP headers to enable the developer to receive, save and return those cookies as needed.

Cookies as simply received from the web service and sent to the web service as headers in the HTTP preamble. In order to use cookies with the ksoap2-android, one needs to save any returned cookies and return them with subsequent calls to the web service.


# Details #

The HttpTransportSE class exposes the method call that, beyond the required SOAP parameters, also accepts a _List_ of _HeaderProperty_ instances. It also returns a like _List_. This provides the ability to append additional headers to the request and review the returned headers. Since a cookie is just one of those header, one can use this facility to send and receive cookies.
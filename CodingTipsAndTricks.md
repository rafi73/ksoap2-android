still missing and waiting for someone to contribute:

  * receiving byte array
  * prevent generation of XML-Tag-Attributes "anyType"
  * receiving and mapping of complex types

and lots more.



# Useful Tools #

There are a bunch of useful generator tools linked in the left hand panel of the website under Links. Try those to generate sample (or production code) for your webservice.

# sending/receiving array of complex types or primitives #

you have to extend Vector an implement KvmSerializable.
see also http://www.blackberry-forum.de/cgi-bin/YaBB.pl?num=1260616413

We want to generate this XML in the Request
```
      <documentIds>
        <string>string</string>
        <string>string</string>
      </documentIds>
      <pluginType>string</pluginType>
      <xmlConfiguration>string</xmlConfiguration>
```

As you see, we want to generate an array with the Propertyname "documentIds" which contains strings with the propertyname "string", and to other simple string properties called "pluginType" and "xmlConfiguration".

for this you have to generate a class like this:
```
import java.util.Hashtable;
import java.util.Vector;

import org.ksoap2.serialization.KvmSerializable;
import org.ksoap2.serialization.PropertyInfo;

public class DocumentIDs extends Vector<String> implements KvmSerializable {

	/**
	 * 
	 */
	private static final long serialVersionUID = -1166006770093411055L;

	@Override
	public Object getProperty(int arg0) {
		return this.get(arg0);
	}

	@Override
	public int getPropertyCount() {
		return 1;
	}

	@Override
	public void getPropertyInfo(int arg0, Hashtable arg1, PropertyInfo arg2) {
		arg2.name = "string";
		arg2.type = PropertyInfo.STRING_CLASS;
	}

	@Override
	public void setProperty(int arg0, Object arg1) {
		this.add(arg1.toString());
	}

}
```

To build the request you have to do this:

make a new Vector-Object from this class:
```
		DocumentIDs documentIdVector = new DocumentIDs();
```

then you can add elements:
```
		documentIdVector.add("any String");
```

then you create a PropertyInfo with it:
```
		documentIdsPropertyInfo = new PropertyInfo();
		documentIdsPropertyInfo.setName("documentIds");
		documentIdsPropertyInfo.setValue(documentIdVector);
		documentIdsPropertyInfo.setType(documentIdVector.getClass());
```

then you add all the properties to the request:
```

		Request = new SoapObject(NAMESPACE, METHOD_NAME);
		Request.addProperty(documentIdsPropertyInfo);
		Request.addProperty("pluginType", "another string");
		Request.addProperty("xmlConfiguration", "next string");
		
		soapEnvelope = new SoapSerializationEnvelope(SoapEnvelope.VER11);
				
		soapEnvelope.setOutputSoapObject(Request);
		
		soapEnvelope.addMapping(NAMESPACE, "documentIds", new DocumentIDs().getClass());
```

When receiving a Response you simply cast the soapEnvelope.bodyIn to the class the response contains. and then access the Vector entries with an index and the values of a complex-type entry with getter-methods.

in the above case this whould be
```
DocumentIDs documentIdResultVector = (DocumentIDs)soapEnvelope.bodyIn;
String resultString = documentIdResultVector.get(0);
```

# How to see raw xml request and response e.g. for debugging? #

Turn debugging on for your httpTransport like so

```
httpTransport.debug = true;
```

and then set a breakpoint at

```
httpTransport.call(soapaction, envelope);
```

inspect the values of

```
httpTransport.requestDump
httpTransport.responseDump
```

# Is there code generation off the WSDL somehow? #

There is the project http://code.google.com/p/wsdl2ksoap/ that uses KSoap2-Android as well as a patch that enables something along these lines in upstream KSoap2 (details on [issue 34](http://code.google.com/p/ksoap2-android/issues/detail?id=34)) however that is all I know about it..

# Marshalling Arrays #

Writing a three dimensional array (as an example) can be done with a Marshaller implemented e.g. like that, reading still needs to be done
```
import org.ksoap2.serialization.Marshal;
import org.ksoap2.serialization.PropertyInfo;
import org.ksoap2.serialization.SoapSerializationEnvelope;
import org.xmlpull.v1.XmlPullParser;
import org.xmlpull.v1.XmlPullParserException;
import org.xmlpull.v1.XmlSerializer;

import java.io.IOException;

public class MarshallArray implements Marshal {
    //this method doesn't work yet
    public Object readInstance(XmlPullParser parser, String namespace, String name, PropertyInfo expected) 
            throws IOException, XmlPullParserException {
        
        return parser.nextText();
    }

    public void register(SoapSerializationEnvelope cm) {
        cm.addMapping(cm.xsd, "String[][][]", String[][][].class, this);

    }

    public void writeInstance(XmlSerializer writer, Object obj) throws IOException {
        String[][][] myArray = (String[][][]) obj;
        for (int i = 0; i < myArray.length; i++) {
            writer.startTag("", "ArrayOfArrayOfString");
            for (int j = 0; j < myArray[i].length; j++) {
                writer.startTag("", "ArrayOfString");
                for (int k = 0; k < myArray[i][j].length; k++) {
                    writer.startTag("", "string");
                    writer.text(myArray[i][j][k]);
                    writer.endTag("", "string");
                }
                writer.endTag("", "ArrayOfString");
            }
            writer.endTag("", "ArrayOfArrayOfString");
        }
    }
}

```

# Manual Parsing of an Array of SoapObjects into an POJO array #

You can create an array of pojos by just parsing through a SoapObjects properties, which are each SoapObjects themselves.  Your response returned from the webservice will be a SoapObject (unless it is very simple - SoapPrimitive then, or it failed.. SoapException then) and you can parse it manually with the various methods like getProperty, getAttribute and so on. See the javadoc for SoapObject for more.

```
ArrayList<Pojo> pojos = null;
int totalCount = soapobject.getPropertyCount();
if (detailsTotal > 0 ) {
    pojos = new ArrayList<Pojo>();
    for (int detailCount = 0; detailCount < totalCount; detailCount++) {
        SoapObject pojoSoap = (SoapObject) soapobject.getProperty(detailCount);
        Pojo Pojo = new Pojo();
        Pojo.idNumber = pojoSoap.getProperty("idNumber").toString();
        Pojo.quantity =  pojoSoap.getProperty("quantity").toString();

        pojos.add(Pojo);
    }
}

```

# How to build up a Element array e.g. for the security header of the request #

See http://stackoverflow.com/questions/4194920/blackberry-ksoap2-soap-header

# Adding an array of complex objects to the request #

To get this xml:
```
<users>
  <user>
     <name>Jonh</name>
     <age>12</age>
  </user>
  <user>
     <name>Marie</name>
     <age>27</age>
  </user>
</users>
```

You would do this:
```
SoapObject users = new SoapObject(NAMESPACE, "users");
SoapObject john = new SoapObject(NAMESPACE, "user");
john.addProperty("name", "john");
john.addProperty("age", 12);
SoapObject marie = new SoapObject(NAMESPACE, "user");
john.addProperty("name", "marie");
john.addProperty("age", 27);
users.addSoapObject(john);
users.addSoapObject(marie);
```

In a similar manner if you have an array of objects or primitives you can build a loop iterating through and adding as above. The approach above works as of 2.5.5. Prior you have to create a SoapObject and add it as a property.

Or look above to "sending/receiving array of complex types or primitives"

You can also add any required attributes to each SoapObject as necessary.

# How to set the SSLSocketFactory on a https connection  in order to allow self-signed certificates read from a KeyStore #

```
public class ConnectionWithSelfSignedCertificate {

	private KeyStore keyStore;

	public ConnectionWithSelfSignedCertificate(KeyStore keyStore) {
		this.keyStore = keyStore;
	}

	public void dummy(String host, int port, String file, int timeout) throws Exception {
		SoapObject client = new SoapObject("", "dummy");
		SoapSerializationEnvelope envelope = new SoapSerializationEnvelope(SoapEnvelope.VER11);
		envelope.bodyOut = client;
		HttpsTransportSE transport = new HttpsTransportSE(host, port, file, timeout);
		((HttpsServiceConnectionSE) transport.getConnection()).setSSLSocketFactory(getSSLSocketFactory());
		transport.call("", envelope);
	}

	private SSLSocketFactory getSSLSocketFactory() throws KeyStoreException, NoSuchAlgorithmException, KeyManagementException {
		TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
		tmf.init(keyStore);
		SSLContext context = SSLContext.getInstance("SSL");
		context.init(null, tmf.getTrustManagers(), null);
		return context.getSocketFactory();
	}
}

```

# Sending a byte array #

To send a byte array (e.g an image) you need to enable MarshalBase64 by adding it as a mapping to the envelope and then add the byte array as a base64 encoded array.

An example is here

http://code.google.com/p/ksoap2-android/issues/detail?id=116

and there are potential performance improvements that can be done in the Android context by using the native Base64 like detailed here

http://code.google.com/p/ksoap2-android/issues/detail?id=82

# Using Basic Authentication #

use the class HttpTransportBasicAuth from the extras package

# Using NTLM Authentication #

use the jar from the ksoap2-extras-ntlm module with the NtlmTransport

# Testing and Debugging in general and specifically for WCF webservices #

Check out the Android application WCF Tester mentioned on ProjectsUsingKsoap2Android as well as client tools like SoapUI.

# Use an array of parameters to create a request #

If you need to use an arrays of parameters, you can use HashMap and just register MarshalHashTable object.
MarshalHashTable is needed to use HashTable object as container of parameters:

Hashtable hashtable = new Hashtable();
hashtable.put("is\_report", false);
hashtable.put("r\_how", 1);
_client.addProperty("params",hashtable);
SoapSerializationEnvelope_envelope = new SoapSerializationEnvelope(SoapEnvelope.VER11);
> _envelope.bodyOut =_client;
HttpTransportSE _ht = new HttpTransportSE("http://www.drebedengi.ru/soap/");_ht.debug = true;
(new MarshalHashtable()).register(_envelope);_

also see the discussion on the mailing list https://groups.google.com/forum/?fromgroups=#!topic/ksoap2-android/OeP-jWVLZMw
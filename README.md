# SeaCat client bridge for Google Volley on Android

The small bridge that enables Android developers to use Google Volley over SeaCat.

More about SeaCat at [TeskaLabs.com](http://teskalabs.com/).
More about Google Voley at http://developer.android.com/training/volley/index.html

## The client bridge

The package name is `com.teskalabs.seacat.android.volley` and it is available in the folder '**/src/main/java**'.

Client bridge depends on SeaCat client (SDK), please contact TeskaLabs support to grab one if you don't have it.

## The example

```java
package example;

import android.os.Bundle;
import android.support.v7.app.ActionBarActivity;
import android.util.Log;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import com.teskalabs.seacat.android.volley.SeaCatStack;

public class VolleySeaCatActivity extends ActionBarActivity
{
    private RequestQueue queue;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Configure Volley to use SeaCat stack
        queue = Volley.newRequestQueue(this, new SeaCatStack());
    }

    void executeURLRequest() {
        // Please note that URL is ending with '.seacat' host postfix
        String stringURL = "http://testhost.seacat/endpoint";

        // Request a string response from the provided URL
        // This is a standard Google Volley request, no SeaCat specific changes are needed
        StringRequest stringRequest = new StringRequest(Request.Method.GET, stringURL,
            new Response.Listener<String>() {
                @Override
                public void onResponse(String response) {
                    int len = response.length();
                    if (len > 400) response = response.substring(0,400);
                    Log.i(LOG, "Response:\n"+response);
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    Log.w(LOG, "That didn't work!\n" + error);
            }
        });

        // Add the request to the RequestQueue
        queue.add(stringRequest);
    }

    String LOG = "example.volleytest";
}
```


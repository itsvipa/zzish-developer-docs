
#Using the Zzish Javascript SDK

###**STEP 1 - Download and Install the SDK**

```
<script src="https://zzish.github.io/zzishsdk-js/zzish.js">
    <script>
        Zzish.init("API_KEY");
        window.onbeforeunload = function (e) {
            Zzish.stop();
        };        
    </script>
```
Use the API KEY you received when you registered your app with Zzish (see developer console).



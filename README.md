# lio-RN-HtmlToPDF

REACT NATIVE HTML to PDF

ORIGINAL REPO :
github:christopherdro/react-native-html-to-pdf

############################################################
ISSUE 1
############################################################
PLATFORM : ANDROID

ISSUE :
-> Cannot access local cache file.

CHANGES in FILE :
-> android/src/main/java/android/print/PdfConverter.java

ISSUE LINK ON GITHUB:-
https://github.com/christopherdro/react-native-html-to-pdf/issues/210

SOLUTION :

1. Given Access in settings to files.

====================================
CHANGE
====================================
// CHANGES START
try {
WebSettings settings = mWebView.getSettings();
settings.setTextZoom(100);
settings.setAllowContentAccess(true);
settings.setAllowFileAccess(true);
settings.setDefaultTextEncodingName("utf-8");
} catch (Exception e) {
Log.d(TAG, "Failed to Access Local File", e);
}
// CHANGES END
====================================
############################################################

############################################################
ISSUE 2
############################################################
PLATFORM : IOS
ISSUE :
-> Cannot access local cache file using htmlstring.

CHANGES in FILE :
-> ios/RNHTMLtoPDF/RNHTMLtoPDF.m

ISSUE LINK ON GITHUB:-
https://github.com/christopherdro/react-native-html-to-pdf/issues/210

SOLUTION :

1. Accessed Cache Directory path.
2. Then generated index.html path using baseURL.
3. Instead of passing html string we are loading html via index.html file which is stored in Cache Directory.
4. Remove cache for old image shown issue in profile picture.

====================================
CHANGE
====================================
// CHANGE START  
 NSArray *paths = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
NSString *cacheDirectory = [paths objectAtIndex:0];
NSURL *baseURL = [NSURL fileURLWithPath:cacheDirectory isDirectory:true];
NSURL *html_path = [NSURL fileURLWithPath:@"index.html" relativeToURL:baseURL];
dispatch_async(dispatch_get_main_queue(), ^{
// REMOVE CACHE FOR OLD IMAGE ISSUE
NSSet *dataTypes = [NSSet setWithArray:@[WKWebsiteDataTypeDiskCache,WKWebsiteDataTypeMemoryCache]];
[[WKWebsiteDataStore defaultDataStore] removeDataOfTypes:dataTypes modifiedSince:[NSDate dateWithTimeIntervalSince1970:0] completionHandler:^{}];
[self->_webView loadFileURL:html_path allowingReadAccessToURL:baseURL];  
 });
// CHANGE END
====================================
############################################################

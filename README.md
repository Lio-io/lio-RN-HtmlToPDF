# lio-RN-HtmlToPDF

REACT NATIVE HTML to PDF

ORIGINAL REPO :
github:christopherdro/react-native-html-to-pdf

ISSUE :
-> Cannot access local cache file using htmlstring.

CHANGES in FILE :
-> ios/RNHTMLtoPDF/RNHTMLtoPDF.m

SOLUTION :

1. Accessed Cache Directory path.
2. Then generated index.html path using baseURL.
3. Instead of passing html string we are loading html via index.html file which is stored in Cache Directory.

====================================
CHANGE
====================================
// CHANGE START  
 NSArray *paths = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES);
NSString *cacheDirectory = [paths objectAtIndex:0];
NSURL *baseURL = [NSURL fileURLWithPath:cacheDirectory isDirectory:true];
NSURL *html_path = [NSURL fileURLWithPath:@"index.html" relativeToURL:baseURL];
dispatch_async(dispatch_get_main_queue(), ^{
[self->_webView loadFileURL:html_path allowingReadAccessToURL:baseURL];
});
// CHANGE END
====================================

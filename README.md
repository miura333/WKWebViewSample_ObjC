# WKWebViewSample_ObjC

WKWebViewのObjective-C版リファレンス

## 宣言
    #import <UIKit/UIKit.h>
    #import <WebKit/WebKit.h>

    @interface MyWebView : UIView <WKNavigationDelegate, WKUIDelegate>
    @property (strong) WKWebView *webView;
    @end

## 初期化
    _webView = [[WKWebView alloc] initWithFrame:CGRectMake(0, 0, 320, 568)];
    _webView.navigationDelegate = self;
    _webView.UIDelegate = self;
    [self addSubview:_webView];
    
## ロード
    [_webView loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:url]]];
    
## 進む、戻る、リロード
    [_webView canGoBack];
    [_webView canGoForward];
    [_webView reload];
    [_webView goForward];
    [_webView goBack];
    
## Delegates

### shouldStartLoadWithRequest
    - (void)webView:(WKWebView *)webView didStartProvisionalNavigation:(WKNavigation *)navigation;
### didStartLoad
    - (void)webView:(WKWebView *)webView didCommitNavigation:(WKNavigation *)navigation;
### didFinishLoad
    - (void)webView:(WKWebView *)webView didFinishNavigation:(WKNavigation *)navigation;
### didFailWithError
    - (void)webView:(WKWebView *)webView didFailNavigation:(WKNavigation *)navigation withError:(NSError *)error;

## Basic認証
UIAlertControllerWrapperはUIAlertControllerをラップしたクラス

    - (void)webView:(WKWebView *)webView didReceiveAuthenticationChallenge:(nonnull NSURLAuthenticationChallenge *)challenge completionHandler:(nonnull void (^)(NSURLSessionAuthChallengeDisposition, NSURLCredential * _Nullable))completionHandler {
        if([challenge.protectionSpace.authenticationMethod isEqualToString:NSURLAuthenticationMethodHTTPBasic] == YES){
            NSLog(@"didReceiveAuthenticationChallenge");
        
            [UIAlertControllerWrapper showAlertViewWithIdAndPassword:@"Authentication" message:@"Input username and password." ok:^(UIAlertAction *action, UIAlertController *cntr){
                NSURLCredential *credential = [NSURLCredential credentialWithUser:[cntr.textFields objectAtIndex:0].text password:[cntr.textFields objectAtIndex:1].text persistence:NSURLCredentialPersistenceForSession];
                completionHandler(NSURLSessionAuthChallengeUseCredential, credential);
            
            }cancel:^(UIAlertAction *cancel){
                completionHandler(NSURLSessionAuthChallengeCancelAuthenticationChallenge, nil);
            }];
        }else{
            completionHandler(NSURLSessionAuthChallengeRejectProtectionSpace, nil);
        }
    }
    
## Javascript実行
    [webView evaluateJavaScript:@"document.title"
              completionHandler:^(NSString *result, NSError *error){
    }];

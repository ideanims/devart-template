##Tweet Components 

Tweets play a key role in my project. So it was important to make sure we didn't run into any showstoppers there. 
So I had to first write the code that gets tweets into the project in sufficient numbers so the dynamic art keeps breathing. 

That was almost straightforward in the sense there were so many sample projects and code out there to pull tweets into your website using their APIs. In our case though, we wanted the raw tweets, so we could run some logic on them. It is after all based on Alice story, known for puzzles and logic play. And we wanted them not on a website, but in a desktop Mac environment.  
Not a problem. Got a sample, tweaked a bit and there, I got my first set of tweets flowing through a Mac/XCode app. 
One thing we may have to do is to build a local buffer of tweets since there are caps imposed by Twitter on API calls. 

Here is some sample code: 

```
#import "TwitterAdapter.h"
#import "AppDelegate.h"

@interface TwitterAdapter ()

@property (strong, nonatomic) ACAccountStore *accountStore;

@end


@implementation TwitterAdapter

- (void)refreshTwitterFeedWithCompletion:(void (^)(NSArray* jsonResponse))completion {
    if(!self.account){
        ACAccountStore* store = [AppDelegate instance].accountStore;
        [self accessTwitterAccountWithAccountStore:store];
        return;
    }
    NSURL* url = [NSURL URLWithString:@"https://api.twitter.com/1.1/statuses/user_timeline.json"];
    NSDictionary* params = @{@"count" : @"50", @"screen_name" : @"rajan2100"};
    SLRequest *request = [SLRequest requestForServiceType:SLServiceTypeTwitter
                                            requestMethod:SLRequestMethodGET
                                                      URL:url parameters:params];
    request.account = self.account;
    [request performRequestWithHandler:^(NSData *responseData,
                                         NSHTTPURLResponse *urlResponse, NSError *error) {
        if (error)
        {
            NSString* errorMessage = [NSString stringWithFormat:@"Error reading Twitter feed. %@",
                                      [error localizedDescription]];
            [[AppDelegate instance] showError:errorMessage];
        }
        else
        {
            NSError *jsonError;
            NSArray *responseJSON = [NSJSONSerialization
                                     JSONObjectWithData:responseData
                                     options:NSJSONReadingAllowFragments
                                     error:&jsonError];
            if (jsonError)
            {
                NSString* errorMessage = [NSString stringWithFormat:@"JSON Error reading Twitter feed. %@",
                                          [jsonError localizedDescription]];
                [[AppDelegate instance] showError:errorMessage];
            }
            else
            {
                dispatch_async(dispatch_get_main_queue(), ^{
                    if(completion){
                        completion(responseJSON);
                    }
                });
            }
        }
    }];
}
@end
```


##Tweet Graphics 

I also worked on the OpenGL part to produce hundreds of those tweet shells and tweet spots you see in this image. They are not drawn to scale. Then we have to add the main animation. That is the fun part.  

![Tweet Strokes](../project_images/DAUpdate3Img.jpg?raw=true "Tweet Strokes")



##Hardware 

On the hardware side, I have tested the microphone and amplifier, but need to pass some audio through the arduino board. To add another dimension to the immersive experience. 

With barely a week to go, I am glad the project is shaping up well. 

I haven't still decided whether the small iPad app would add value to the project. I have something working, but let us see how the rest of the project shapes up. 

Another update in 2 days and then it is time to wrap it all up. Looking forward to an exciting finish line. 




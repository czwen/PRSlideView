# PRSlideView

[![CocoaPods](https://img.shields.io/cocoapods/v/PRSlideView.svg)](https://cocoapods.org/pods/PRSlideView)
[![Language](https://img.shields.io/badge/language-Objective--C-blue.svg)](../../search)
[![License](https://img.shields.io/github/license/Elethom/PRSlideView.svg)](/LICENSE)

[![Tweet](https://img.shields.io/twitter/url/http/ElethomHunter.svg?style=social)](https://twitter.com/intent/tweet?text=PRSlideView%3A%20Slide%20view%20with%20gracefully%20written%20UIKit-like%20APIs.&url=https%3A%2F%2Fgithub.com%2FElethom%2FPRSlideView&via=ElethomHunter)
[![Twitter](https://img.shields.io/twitter/follow/ElethomHunter.svg?style=social)](https://twitter.com/intent/follow?user_id=1512633926)

Slide view with gracefully written UIKit-like methods, delegate and data source protocol.

Note: Auto layout not supported due to the special behaviours of `UIScrollView`. Please use autoresizing mask instead or wrap it with a container view.

## Features

* Horizontal or vertical scrolling
* Infinite scrolling
* Page control (horizontal mode only)

## Installation

### With CocoaPods

In your `Podfile`:

```Ruby
pod 'PRSlideView'
```

## Usage

### Create a Slide View

```Objective-C
PRSlideView *slideView = [[PRSlideView alloc] initWithFrame:self.view.bounds];
slideView.delegate = self;
slideView.dataSource = self;
slideView.direction = PRSlideViewDirectionHorizontal; // horizontal by default
slideView.infiniteScrollingEnabled = YES; // disabled by default
slideView.showsPageControl = YES; // enabled by default
[slideView registerClass:PRAlbumPage.class
  forPageReuseIdentifier:NSStringFromClass(PRAlbumPage.class)];
slideView.autoresizingMask = (UIViewAutoresizingFlexibleWidth |
                              UIViewAutoresizingFlexibleHeight);
self.slideView = slideView;
[self.view addSubview:slideView];
```

### Create a Slide View Page Subclass

```Objective-C
#import "PRSlideViewPage.h"

@interface PRAlbumPage : PRSlideViewPage

@property (nonatomic, weak) UIImageView *coverImageView;

@end
```

```Objective-C
#import "PRAlbumPage.h"

@implementation PRAlbumPage

- (instancetype)initWithFrame:(CGRect)frame
{
    self = [super initWithFrame:frame];
    if (self) {
        UIImageView *coverImageView = [[UIImageView alloc] initWithFrame:self.bounds];
        coverImageView.autoresizingMask = (UIViewAutoresizingFlexibleWidth |
                                           UIViewAutoresizingFlexibleHeight);
        coverImageView.contentMode = UIViewContentModeScaleAspectFit;
        self.coverImageView = coverImageView;
        [self addSubview:coverImageView];
    }
    return self;
}
```

### Use Data Source

```Objective-C
#pragma mark - PRSlideViewDataSource

- (NSInteger)numberOfPagesInSlideView:(PRSlideView *)slideView
{
    return self.albumData.count;
}

- (PRSlideViewPage *)slideView:(PRSlideView *)slideView pageAtIndex:(NSInteger)index
{
    PRAlbumPage *page = [slideView dequeueReusablePageWithIdentifier:NSStringFromClass(PRAlbumPage.class)
                                                            forIndex:index];
    
    NSString *imageName = [self.albumData[index] stringByAppendingPathExtension:@"jpg"];
    page.coverImageView.image = [UIImage imageNamed:imageName];
    
    return page;
}
```

### Use Delegate

```Objective-C
#pragma mark - PRSlideViewDelegate

- (void)slideView:(PRSlideView *)slideView didScrollToPageAtIndex:(NSInteger)index
{
    self.titleLabel.text = self.albumData[index];
}

- (void)slideView:(PRSlideView *)slideView didClickPageAtIndex:(NSInteger)index
{
    UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"You clicked an album"
                                                    message:self.albumData[index]
                                                   delegate:nil
                                          cancelButtonTitle:@"OK"
                                          otherButtonTitles:nil];
    [alert show];
}
```

All done! You can check out the code in the demo provided.

## License

This code is distributed under the terms and conditions of the [MIT license](http://opensource.org/licenses/MIT).

## Donate

You can support me by:

* sending me iTunes Gift Cards;
* via [Alipay](https://www.alipay.com): elethomhunter@gmail.com
* via [PayPal](https://www.paypal.com): elethomhunter@gmail.com

:-)

## Contact

* [Telegram](http://telegram.me/elethom)
* [Email](mailto:elethomhunter@gmail.com)
* [Twitter](https://twitter.com/elethomhunter)
* [Blog](http://blog.projectrhinestone.org)


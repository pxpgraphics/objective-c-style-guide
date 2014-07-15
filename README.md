# PXPGraphics Objective-C Style Guide

This style guide outlines the coding conventions of the iOS team at PXPGraphics. The reason we made this style guide was so that we could keep the code in our blogs, documentation, projects and SDKs nice and consistent.

## Introduction

These guidelines build on Apple's existing guides.  Unless explicitly contradicted below, assume that all of Apple's guidelines apply as well. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [Programming with Objective-C](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Adopting Modern Objective-C](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/ModernizationObjC/AdoptingModernObjective-C/AdoptingModernObjective-C.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](https://developer.apple.com/library/ios/documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Credits

The creation of this style guide was implemented by [Paris Xavier Pinkney](https://github.com/pxpgraphics).

We would like to thank the creators of the [GitHub](https://github.com/github/objective-c-style-guide), [Ray Wenderlich](https://github.com/raywenderlich/objective-c-style-guide), [New York Times](https://github.com/NYTimes/objective-c-style-guide), and [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)' Objective-C Style Guides. These four style guides provided a solid starting point for this guide to be created and based upon.

## Table of Contents

* [Language](#language)
* [Code Organization](#code-organization)
* [Spacing](#spacing)
* [Comments](#comments)
* [Naming](#naming)
  * [Underscores](#underscores)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Case Statements](#case-statements)
* [Private Properties](#private-properties)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Init & Dealloc Methods](#init-and-dealloc-methods)
* [Class Constructor Methods](#class-constuctor-methods)
* [CGRect Functions](#cgrect-functions)
* [Golden Path](#golden-path)
* [Lifetime Qualifiers](#lifetime-qualifiers)
* [Error Handling](#error-handling)
* [Singletons](#singletons)
* [Copyright](#copyright)
* [Xcode Project](#xcode-project)

## Language

US English should be used.

**Preferred:**
```objc
UIColor *myColor = [UIColor blueColor];
```

** Not Preferred:**
```objc
UIColor *myColour = [UIColor blueColor];
```

## Code Organization

Use `#pragma mark - `to categorize method in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)viewWillDisappear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (id)customProperty {}
- (void)setCustomProperty:(id)value {}

#pragma mark - IBActions

- (IBAction)doneAction:(id)sender {}

#pragma mark - Public Methods

- (void)publicMethod {}

#pragma mark - Private Methods

- (void)privateMethod {}

#pragma mark - Protocol Conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode (Xcode defaults to 4 spaces).
* Colon-aligning method invocation should often by avoided. There are cases where a method signature may have >= 3 colons and colon-aligning make the code more readable. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes is illegible.

**Preferred:**
```objc
// Block are easily readable.
[UIView animateWithDuration: 1.0 animations:^{
    // Do something.
} completion:^(BOOL finished) {
    // Do something.
}];

```

** Not Preferred:**
```objc
// Colon-aligning makes the block indentation wacky and difficult to read.
[UIView animateWithDuration:1.0
                 animations:^{
                    // Do something.
               } completion:^(BOOL finished) {
                    // Do something.
            }];
```

* Method braces always open and close on a new line.

**Preferred:**
```objc
// A method's content is more easily readable with the opening bracket on a new line.
- (void)viewDidLoad
{
    [super viewDidLoad];
    // Do something.
}
```

**Not Preferred:**
```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do something.
}
```

* Other braces (e.g., `if`/`else`/`switch`/`while`, etc.) always open on the same line as the statement but close on a new line.

**Preferred:**
```objc
// Conditional statements are easier to read when using Egyptian brackets.
if (user.isHappy) {
    // Do something.
} else {
    // Do something else.
}
```

**Not Preferred:**
```objc
if (user.isHappy)
{
    // Do something.
}
else {
    // Do something else.
}
```

## Comments

When they are needed, comments should be user to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. *Exception: This does not apply to those comments used to generate documentation.*

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/questions/2865185/do-you-need-to-release-parameters-of-methods-at-the-end-of-them-in-objective-c/2865194#2865194)).

Long, descriptive method and variable names are good.

**Preferred:**
```objc
UIButton *settingsButton;
```

**Not Preferred:**
```objc
UIButton *setBut;
```

A two-to-three letter prefix should always be used for class names and constants, however, may be omitted for Core Data entity names. For any official PXPGraphics SDKs, the prefix 'PXP' should be used.

Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**Preferred:**
```objc
static NSTimeInterval const PXPLogInViewControllerFadeAnimationDuration = 0.3;
```

**Not Preferred:**
```objc
static NSTimeInterval const fadeDuration = 1.7;
```

Properties should be camel-case with the leading word being lowercase.

**Preferred:**
```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**Not Preferred:**
```objc
id varnm;
```

## Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`.

An exception to this: inside initializers, the backing instance variable (i.e., _variableName) should be used directly to avoid potential side effects of the getters/setters.

Local variables should not contain underscores.

## Methods

In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.

The usage of the work "and" is reserved. It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

**Preferred:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**
```objc
- (void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height; // Never do this.
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable (e.g., `NSString *text`, not `NSString* text` or `NSString * text`), except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent.

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (`init`, `initWithCoder:`, etc.), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Preferred:**
```objc
@interface PXPUser : NSObject

@property (strong, nonatomic) NSString *username;
@property (strong, nonatomic) NSInteger userId;

@end
```

**Not Preferred:**
```objc
@interface PXPUser : NSObject {
    NSString *username;
    NSInteger userId;
}
```

## Property Attributes

Property attributes should be explicitly listed, and will help remote programmers when reading the code. The order of properties should be storage then atomicity, which is consistent with automatically generated code when connection UI elements from Interface Builder or code snippets.

**Preferred:**
```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (nonatomic) CGFloat containerViewHeight;
@property (strong, nonatomic) NSString *brandName;
```

**Not Preferred:**
```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) CGFloat containerViewHeight;
@property (nonatomic) NSString *brandName;
```

Properties with mutable counterparts (e.g., `NSString`) should prefer `copy` instead of `strong`. Why? Even if you declared a property as `NSString` someone might pass in an instance of an `NSMutableString` and then change it without you noticing.

**Preferred:**
```objc
@property (copy, nonatomic) NSString *firstName;
@property (copy, nonatomic) NSString *lastName;
```

**Not Preferred:**
```objc
@property (strong, nonatomic) NSString *firstName;
@property (strong, nonatomic) NSString *lastName;
```

## Dot-Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you use dot syntax, the property is still accessed or changed using getter and setter methods. Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html).

Dot-notation should **always** be used for accessing and mutating properties, as it makes code more concise. Bracket notation is preferred in all other instances.

**Preferred:**
```objc
NSUInteger arrayCount = [self.array count];
self.view.backgroundColor = [UIColor blueColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**
```objc
NSUInteger arrayCount = self.array.count;
[self.view setBackgroundColor:[UIColor blueColor]];
UIApplication.sharedApplication.delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever create immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

Please be sure to place spaces between the literal operators and the content to increase legibility.

**Preferred:**
```objc
NSArray *cities = @[ @"Emeryville, Oakland, Berkeley, San Francisco, Marin, San Mateo" ];
NSDictionary *engineers = @{ "iOS": @"Paris", @"OSX": @"Zack" };
NSNumber *shouldUseLiterals = @YES;
NSNumber *zipCode = @94608
```

**Not Preferred:**
```objc
NSArray *cities = [NSArray arrayWithObjects:@"Emeryville, Oakland, Berkeley, San Francisco, Marin, San Mateo"];
NSDictionary *engineers = [NSDictionary dictionaryWithObjectsAndKeys:@"Paris", @"iOS", @"Zack", @"OSX"];
NSNumber *shouldUserLiterals = [NSNumber numberWithBool:YES];
NSNumber *zipCode = [NSNumber numberWithInteger:94608];
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**Preferred:**
```objc
static NSString * const PXPAboutViewControllerCompanyName = @"PXPGraphics, Inc."
static CGFloat const PXPImageThumbnailHeight = 50.0;
```

**Not Preferred:**
```objc
#define CompanyName @"PXPGraphics, Inc."
#define thumbnailHeight 50
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now included a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`.

**For Example:**
```objc
typdef NS_ENUM(NSUInteger, PXPMenuItemType) {
    PXPMenuItemHome,
    PXPMenuItemProfile,
    PXPMenuItemSettings
}
```

You can also make explicit value assignments (showing older k-style constant definition):
```objc
typedef NS_ENUM(NSUInteger, PXPGlobalConstants) {
    PXPAnnotationSizeMin = 1,
    PXPAnnotationSizeMax = 5,
    PXPClusterCountMin = 100,
    PXPClusterCountMax = 500
}
```

Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

**Not Preferred:**
```objc
enum GlobalConstants {
    kMaxAnnotationSize = 5,
    kMaxClusterCount = 500
}
```

## Bitmasks

When working with bitmasks, use the new fixed underlying type specification `NS_OPTIONS`.

**For Example:**
```objc
typedef NS_OPTIONS(NSUInteger, PXPArticleCategory) {
    PXPArticleCategoryAutomotive = 1 << 0,
    PXPArticleCategoryDining = 1 << 1,
    PXPArticleCategoryGrocery = 1 << 2,
    PXPArticleCategoryKids = 1 << 3,
    PXPArticleCategoryPets = 1 << 4
}
```

## Case Statements

Braces are not required for case statements, unless enforced by the compiler. When a case contains more than one line, braces should be added.

```objc
switch (condition) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Multi-line example using braces.
        break;
    }
    case 3:
        // ...
        break;
    default:
        // ...
        break;
}
```

There are times when the same code can be used for multiple cases, and a fall-through should be used. A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value. A fall-through should be commented for coding clarity.

```objc
switch (condition) {
    case 1:
        // ** fall-through! **
    case 2:
        // code executed for values 1 and 2
        break;
    default:
        // ...
        break;
}
```

When using an enumerated type for a switch, 'default' is not needed. For example:
```objc
PXPMenuItemType menuType = PXPMenuItemHome;

switch (menuType) {
    case PXPMenuItemHome:
        // ...
        break;
    case PXPMenuItemProfile:
        // ...
        break;
    case PXPMenuItemSettings:
        // ...
        break;
}
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `PXPPrivate` or `private`) should never be used unless extending another class. The Anonymous category can be shared/exposed for testing using the +Private.h file naming convention.

**For Example:**
```objc
@interface PXPMapViewController ()

@property (strong, nonatomic) MKMapView *mapView;
@property (strong, nonatomic) UITableView *listView;

@end
```

## Booleans

Objective-C uses `YES` and `NO`. Therefore `true` and `false` should only be used for CoreFoundation, C, or C++ code. Since `nil` resolves to `NO`, it is unnecessary to compare it in conditions, Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**Preferred:**
```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**NOt Preferred:**
```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the 'is' prefix, but specifies the conventional name for the get accessor, for example:
```objc
@property (assign, getter=isEditable) BOOL editable;
```

Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, even more dangerous defect may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**Preferred:**
```objc
if (!error) {
    return success;
}
```

**Not Preferred:**
```objc
if (!error)
    return success;
```

or
```objc
if (!error) return success;
```

## Ternary Operator

The Ternary operator, `?:`, should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability. If the variable being compared is a boolean type, then no parentheses are needed.

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;

NSString *firstName = nil;
result = [NSString stringWithFormat:@"Hello %@", firstName ?: @"World!"] // Prints 'Hello World!' because firstName is nil.
```

## Init & Dealloc Methods

Dealloc methods, `dealloc`, should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `Init` should be placed directly below the `dealloc` methods of any class.

Init methods, `init`, should follow the convention provided by Apple's generated code template. A return type of `instancetype` should also be used instead of `id`.

**For Example:**
```objc
- (void)dealloc
{
    [NSNotificationCenter defaultCenter] removeObserver:self];
}

- (instancetype)init
{
    self = [super init];
    if (self) {
        // ...
    }
    return self;
}
```

See [Class Constructor Methods](#class-constructor-methods) for link to an article on `instancetype`.

## Class Constructor Methods

Where class constructor methods are used, these should always return type of `instancetype` and never `id`.
This ensures the compiler correctly infers the result type.

```objc
@interface PXPUser

+ (instancetype)userWithType:(PXPUserType)type;

@end
```

More information on `instancetype` can be found on [NSHipster](http://nshipster.com/instancetype/)

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:
> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**
```objc
CGRect bounds = self.bounds

CGFloat xOrigin = CGRectGetMinX(bounds);
CGFloat yOrigin = CGRectGetMinY(bounds);
CGFloat width = CGRectGetWidth(bounds);
CGFloat height = CGRectGetHeight(bounds);

CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**
```objc
CGRect bounds = self.bounds;

CGFloat xOrigin = bounds.origin.x;
CGFloat yOrigin = bounds.origin.y;
CGFloat width = bounds.size.width;
CGFloat height = bounds.size.height;

CGRect frame = (CGRect){ .origin = CGPointZero, .size bounds.size}
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the 'golden' or 'happy' path. That is, don't nest `if` statements. Multiple return statements are OK.

**Preferred:**
```objc
- (void)someMethod
{
    if (![someOther boolValue]) {
        return;
    }

    // Do something important.
}
```

**Not Preferred:**
```objc
- (void)someMethod
{
    if ([someOther boolValue]) {
        // Do something important.
    }
}
```

## Lifetime Qualifiers

You should use [lifetime qualifiers](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html) to avoid strong reference cycles. For blocks in particular, you should use a temporary `__weak` variable.

**Preferred:**
```objc
// self is an instance of PXPMainViewController.
__typeof__(self) __weak weakSelf = self;
self.completionHandler = ^(NSInteger result) {
    [weakSelf dismissViewControllerAnimated:YES completion:nil];
};
```

**Not Preferred:**
```objc
// self is an instance of PXPMainViewController.
PXPMainViewController * __weak weakSelf = self;
self.completionHandler = ^(NSInteger result) {
    [weakSelf dismissViewControllerAnimated:YES completion:nil];
};
```

## Error Handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle error.
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle error.
}
```

Some of Apple's APIs write garbage values to the error parameter (if non-`NULL`) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.

```objc
+ (instancetype)sharedInstance
{
    static __typeof__(self) sharedInstance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[self alloc] init];
    });
    return sharedInstance;
}
```

This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Copyright

By default, use a PXPGraphics, Inc. copyright. There may be special circumstances where it should be changed to a customer and/or parter name.

```objc
//  Copyright (c) 2014 PXPGraphics, Inc. All rights reserved.
```

Set the Organization in your .xcodeproj to override your default company name for new files that are created. Select the project in File Navigator, then use the File Inspector (first button) in the Utilities pane. Set the organization to PXPGraphics, Inc.

## Xcode Project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many additional warnings as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

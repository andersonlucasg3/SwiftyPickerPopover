# SwiftyPickerPopover
A more convenient way to display a popover with a built-in picker, on iPhone/iPad of iOS9+.

[![Version](https://img.shields.io/cocoapods/v/SwiftyPickerPopover.svg?style=flat)](http://cocoadocs.org/docsets/SwiftyPickerPopover) 
[![License](https://img.shields.io/cocoapods/l/SwiftyPickerPopover.svg?style=flat)](http://cocoadocs.org/docsets/SwiftyPickerPopover)
[![Platform](https://img.shields.io/cocoapods/p/SwiftyPickerPopover.svg?style=flat)](http://cocoadocs.org/docsets/SwiftyPickerPopover)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/hsylife/SwiftyPickerPopover)

## Features
- By simple code, you can display a popover that contains a built-in picker, on iPhone or iPad.
- Swift 3, iOS9+. UIPopoverController free. 
- Callback

## Screenshots
<img src="README_resources/SwiftyPickerPopover_movie.gif" width="362">

## Required
- Swift 3, Xcode 8.
- iOS 9+
- CocoaPods 1.1.0.rc.2+ or Carthage 0.12.0+

## License
MIT.

## Usage
For installing it with CocoaPods, specify it in your 'Podfile':
```ruby
platform :ios, '9.0'
use_frameworks!
pod 'SwiftyPickerPopover'
```
Run 'pod install'.

Instead, for installing it with Carthage, add it to your Cartfile:
```
github "hsylife/SwiftyPickerPopover"
```
Run 'carthage update --platform iOS'.

On Xcode, import the module:
```swift
import SwiftyPickerPopover
```
### Basic
To display a popover with an UIDatePicker, the code looks like this:
```swift
DatePickerPopover(title: "DatePicker")
            .appear(originView: sender, baseViewController: self)
```

To specify more arguments:
```swift
DatePickerPopover(title: "DatePicker")
            .setDateMode(.date)
            .setSelectedDate(Date())
            .setDoneAction({ popover, selectedDate in print("selectedDate \(selectedDate)")})
            .setCancelAction({ popover in print("cancel")})
            .appear(originView: sender, baseViewController: self)
```

To display a popover with an UIPickerView that allows users to choose a String type choice:
```swift
StringPickerPopover(title: "StringPicker", choices: ["value 1","value 2","value 3"])
        .setSelectedRow(0)
        .setDoneAction({ (popover, selectedRow, selectedString) in
            print("done row \(selectedRow) \(selectedString)")
        })
        .setCancelAction( { popover in print("cancel")}
        )
        .appear(originView: button, baseViewController: self)
```

To display a popover with an UIPickerView of multiple columns:
```swift
ColumnStringPickerPopover(title: "Columns Strings",
                                  choices: [["Breakfast", "Lunch", "Dinner"],["Tacos", "Sushi", "Steak", "Waffles", "Burgers"]],
                                  selectedRows: [0,0], columnPercents: [0.5, 0.5])
        .setDoneAction({ popover, selectedRows, selectedStrings in print("selected rows \(selectedRows) strings \(selectedStrings)")})
        .setCancelAction({p in print("cancel")})
        .setFontSize(14)
        .appear(originView: sender, baseViewController: self)
)
```

To display a popover with an UIDatePicker of countDownTimer style:
```swift
 CountdownPickerPopover(title: "CountdownPicker")
            .setSelectedTimeInterval(TimeInterval())
            .setDoneAction({ popover, timeInterval in print("timeInterval \(timeInterval)")} )
            .setCancelAction({ popover in print("cancel")})
            .setClearAction({ popover in print("Clear")
                popover.setSelectedTimeInterval(TimeInterval()).reload()
            })
            .appear(originView: sender, baseViewController: self)
```

### Advanced
To display a DatePickerPopover has a clear button, which rewinds itself by tapping the button, and.which disappers automatically after a certain number of seconds:
```swift
let p = DatePickerPopover(title: "Clearable DatePicker")
            .setDoneAction({ popover, selectedDate in print("selectedDate \(selectedDate)")} )
            .setCancelAction({ popover in print("cancel")})
            .setClearAction({ popover in
                print("clear")
                //Rewind
                popover.setSelectedDate(Date()).reload()
            })
            
        p.appear(originView: sender, baseViewController: self)
        p.disappearAutomatically(after: 3.0)
```

To display a DatePickerPopover of .time style with a time interval of 5 mins and the arrow only to .down direction permitted:
```swift
DatePickerPopover(title: "DatePicker .time 5minInt.")
            .setDateMode(.time)
            .setMinuteInterval(5)
            .setPermittedArrowDirections(.down)
            .setDoneAction({ popover, selectedDate in print("selectedDate \(selectedDate)")} )
            .setCancelAction({ popover in print("cancel")})
            .appear(originView: sender, baseViewController: self)
)
```

To display a StringPickerPopover which can prepare a choice string for display on picker separately from a source string:
```swift
let displayStringFor:((String?)->String?)? = { string in
   if let s = string {
      switch(s){
      case "value 1":
        return "😊"
      case "value 2":
         return "😏"
      case "value 3":
         return "😓"
      default:
         return s
      }
    }
  return nil
  }
        
let p = StringPickerPopover(title: "StringPicker", choices: ["value 1","value 2","value 3"])
            .setDisplayStringFor(displayStringFor)
            .setDoneAction({
                popover, selectedRow, selectedString in
                print("done row \(selectedRow) \(selectedString)")
            })
            .setCancelAction({ popover in
                print("cancel")
            })
            
        p.appear(originView: sender, baseViewController: self)
        p.disappearAutomatically(after: 3.0, completion: { print("automatically hidden")} )
```

A StringPickerPopover appears from the collectionView's cell:
```swift
func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
        
        let theCell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
        
        let p = StringPickerPopover(title: "CollectionView", choices: ["value 1","value 2","value 3"])
                        .setSelectedRow(1)
                        .setDoneAction({ (popover, selectedRow, selectedString) in
                              print("done row \(selectedRow) \(selectedString)")
                         })
                        .setCancelAction( { popover in print("cancel")} )
        
        p.appear(originView: theCell, baseView: collectionView, baseViewController: self)
        
    }
```

## Contributors
Ken Torimaru  [GitHub](https://github.com/ktorimaru) for CountdownPickerPopover and ColumnStringPickerPopover.

BalestraPatrick [GitHub](https://github.com/BalestraPatrick)

## Author
Yuta Hoshino [Twitter](https://twitter.com/hsylife) [Facebook](https://www.facebook.com/yuta.hoshino)

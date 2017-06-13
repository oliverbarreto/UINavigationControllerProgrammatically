# How to work with UINavigationControllers programmatically
How to create an app that presents UINavigationController Programmatically from another UIViewController (not the root view controller in AppDelegate)


## First the Theory
You can add UINavigationController like below:

First you have to create object of SecondViewController,
```swift
let objVC: SecondViewController? = storyboard?.instantiateViewController(withIdentifier: "SecondViewController")
```
Or
```swift
let objVC: SecondViewController? = SecondViewController(nibName: "SecondViewController", bundle: nil)
```
Or
```swift
let objVC: SecondViewController? = SecondViewController()
``
Then add Navigation to SecondViewController
```swift
let aObjNavi = UINavigationController(rootViewController: objVC!)
```
If you want to present then use :
```swift
self.present(aObjNavi, animated: true) {
}
```
If you want to push then use :
```swift
self.navigationController?.pushViewController(aObjNavi, animated: true)
```
If you want to set as root controller then use :
```swift
let appDelegate: AppDelegate = (UIApplication.shared.delegate as? AppDelegate)!
appDelegate.window?.rootViewController = aObjNavi
```

The thing is that if you create the UINavigationController and you set it as the initial ViewController in AppDelegate, you don't need to do anything else than that... 

However,if you create the UINAviogationCobntroller and set it up for the first time from another ViewController, let's say, from a regular UIViewController, you need to create and format the NavigationBar:

  * First, specify the NavigationItem.title to set a title
  * Second, create a done or dismissh button as a NavigationItem.LeftButtonItem

## The Working Example
First we put a normal ViewController in AppDelegate
```swift
window = UIWindow(frame: UIScreen.main.bounds)
        window?.makeKeyAndVisible()
        
        // Create FirstViewController
        let regularViewController = FirstViewController()
        window?.rootViewController = regularViewController
```

This is how we present the UINavigationController with another VC.

```swift
        // Present SecondVC in a newly created UINavigationController
        let secondaryVC = SecondViewController()
        let navVC = UINavigationController(rootViewController: secondaryVC)
        self.present(navVC, animated: true)
```

The secondVC is now resopnsible for showing the baritems

```swift
        // Customize NavigationBar
        navigationItem.title = "Secondary"
        
        // Creates the LeftBarButtonItem:
        let backButton: UIBarButtonItem = UIBarButtonItem(title: "done", style: .done, target: self, action: #selector(backbuttonPressed))
        self.navigationItem.leftBarButtonItem = backButton
        
        // Handles the LeftBarButtonItem: This is needed in order to dismish the current Controller which is the first controller in UINavigation Stack, which is the initialVC in the stack
        func backbuttonPressed() {
            self.dismiss(animated: true, completion: nil)
        }
```

From this secondVC you can push and pop more ViewControllers as desired:

```swift
        // Push ThridVC into the UINavigationController Stack
        let thirdyVC = ThirdViewController()
        self.navigationController?.pushViewController(thirdyVC, animated: true)
``


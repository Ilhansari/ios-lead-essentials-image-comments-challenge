# The Image Comments Challenge - iOSLeadEssentials.com

![](https://github.com/essentialdevelopercom/ios-lead-essentials-image-comments-challenge/workflows/CI-iOS/badge.svg) ![](https://github.com/essentialdevelopercom/ios-lead-essentials-image-comments-challenge/workflows/CI-macOS/badge.svg)

It’s time to put your development skills to the test! 

You are called to add a new feature to the Feed App: displaying image comments when a user taps on an image in the feed.

The goal is to implement this feature using what you learned in the program.

You'll develop the API, Presentation, and UI layers for this feature.

*Important: There's no need to cache comments.*


## Goals

1) Display a list of comments when the user taps on an image in the feed.

2) Loading the comments can fail, so you must handle the UI states accordingly. 
	- Show a loading spinner while loading the comments.
		
		- If it fails to load: Show an error message.
		
		- If it loads successfully: Show all loaded comments in the order they were returned by the remote API.

3) The loading should start automatically when the user navigates to the screen.
	- The user should also be able to reload the comments manually (Pull-to-refresh).

4) At all times, the user should have a back button to return to the feed screen.
	- Cancel any running comments API requests when the user navigates back.
		- Important: Only cancel when the scene has been deallocated.
		- **Don't** cancel the request on `viewWillDisappear`/`viewDidDisappear` because it doesn't mean the view has been deallocated (the view may appear again!).

5) The comments screen layout should match the UI specs.
	- Present the comment date using relative date formatting, e.g., "1 day ago."

6) The comments screen title should be localized in all languages supported in the project.

7) The comments screen should support:
	- Light and Dark Mode
	- Dynamic Type (Scalable font sizes based on the user preference)

8) Write tests to validate your implementation, including unit, integration, and snapshot tests (aim to write the test first!).

9) Follow the specs below and test-drive this feature from scratch:

---

## UI Specs

Follow the UI specs for loading, error, and success states:

![Image Comments UI](image-comments-ui.png)

![Image Comments UI](image-comments-ui-specs.png)

---

## BDD Specs

### Story: Image Comments

### Narrative

```
As an online customer
I want the app to load image commments
So I can see how people are engaging with images in my feed
```

#### Scenarios (Acceptance criteria)

```
Given the customer has connectivity
 When the customer requests to see comments on an image
 Then the app should display all comments for that image
```

```
Given the customer doesn't have connectivity
 When the customer requests to see comments on an image
 Then the app should display an error message
```

## Use Cases

### Load Image Comments From Remote Use Case

#### Data:
- ImageID

#### Primary course (happy path):
1. Execute "Load Image Comments" command with above data.
2. System loads data from remote service.
3. System validates data.
4. System creates comments from valid data.
5. System delivers comments.

#### Invalid data – error course (sad path):
1. System delivers invalid data error.

#### No connectivity – error course (sad path):
1. System delivers connectivity error.

---

## Model Specs

### Feed Image Comment

| Property          | Type                    |
|-------------------|-------------------------|
| `id`              | `UUID`                  |
| `message` 	    | `String`			      |
| `created_at`      | `Date` (ISO8601 String) |
| `author` 			| `CommentAuthorObject`   |

### Feed Image Comment Author

| Property          | Type                |
|-------------------|---------------------|
| `username` 	    | `String`			  |

### Payload contract

```
GET /image/{image-id}/comments

2xx RESPONSE (**Important**: any 2xx response is valid - not only 200!)

{
	"items": [
		{
			"id": "a UUID",
			"message": "a message",
			"created_at": "2020-05-20T11:24:59+0000",
			"author": {
				"username": "a username"
			}
		},
		{
			"id": "another UUID",
			"message": "another message",
			"create at": "2020-05-19T14:23:53+0000",
			"author": {
				"username": "another username"
			}
		},
		...
	]
}
```

#### Base URL

https://ile-api.essentialdeveloper.com/essential-feed

#### Feed URL

Base URL + /v1/feed

https://ile-api.essentialdeveloper.com/essential-feed/v1/feed

#### Image Comments URL

Base URL + /v1/image/{image-id}/comments

https://ile-api.essentialdeveloper.com/essential-feed/v1/image/{image-id}/comments

---

## Instructions

1) Fork the latest version of this repository. Here's <a href="https://guides.github.com/activities/forking" target="_blank">how forking works</a>.

2) Open the `EssentialApp/EssentialApp.xcworkspace` workspace project on Xcode 12.4 (you can use other Xcode versions by switching to the appropriate branch, e.g., `xcode11`, `xcode12`, `xcode12_2`, `xcode12_3`).

	- ⚠️ Important: ***Different simulators may generate slightly different snapshots (even if they look the same!).*** So you must run the snapshot tests using the appropriate simulator based on the branch you chose:

		- `xcode11` -> iPhone 11 - iOS 13.x
		- `xcode12` -> iPhone 12 - iOS 14.0 or iOS 14.1
		- `xcode12_2` -> iPhone 12 - iOS 14.2
		- `xcode12_3` -> iPhone 12 - iOS 14.3
		- `xcode12_4` -> iPhone 12 - iOS 14.4

	- Do not change the indentation in the project.

	- Do not rename the existing classes and files.

	- Important: Every time you build the project, it'll automatically reformat the modified files with SwiftFormat to maintain the code consistent.

3) You can develop the platform-agnostic logic in the `EssentialFeed` target using the `macOS` platform to speed up the TDD cycle.

4) Feel free to organize the 'Image Comments' feature in any way you want in the project. You can use the existing projects and targets, or create new ones if you want to.
	
	- If you add new projects, make sure to add them to the `EssentialApp` workspace.

	- If you add new targets, make sure to add them to the `CI_macOS` and `CI_iOS` schemes as needed, so we can run all tests on the CI server.

5) You can see/interact with your solution by running the Application on the simulator (or device). 
	- Switch to the `EssentialApp` scheme and press CMD+R.

6) Errors should be handled accordingly.

	- There shouldn't be *any* `fatalError` in production code.

	- There shouldn't be empty `catch` blocks.

	- There shouldn't be any `print` statements, such as `print(error)`.

7) When all tests are passing and you're done implementing your solution:

	- Create a Pull Request from your branch to the main challenge repo's matching branch.

		- For example, if you implemented the challenge using the `xcode12_4` branch, your PR should be from your fork's `xcode12_4` branch into the main repo's `xcode12_4` branch (DO NOT MIX Xcode versions or you'll have merge conflicts!).

	- The title of the Pull Request should be: **Your Name - Image Comments Challenge**.

8) Post a comment in the challenge page in the academy with the link to your PR, so we can review your solution and provide feedback.


## Guidelines


1) Aim to commit your changes every time you add/alter the behavior of your system or refactor your code.

2) Aim for descriptive commit messages that clarify the intent of your contribution which will help other developers understand your train of thought and purpose of changes.

3) The system should always be in a green state, meaning that in each commit all tests should be passing.

4) The project should build without warnings.

5) The code should be carefully organized and easy to read (e.g. indentation must be consistent).

6) Make careful and proper use of access control, marking as `private` any implementation details that aren’t referenced from other external components.

7) Aim to write self-documenting code by providing context and detail when naming your components, avoiding explanations in comments.

Happy coding!

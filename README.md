# Firebase Authentication Lab

### We will be using Firebase's Authentication product to impliment signing up new users, and logging in users.
---

There are many ways to authenticate users, we are going to use basic password auth.
[Password Auth Docs](https://firebase.google.com/docs/auth/web/password-auth)

## 0. Before you start, update your code.
### We have some sensitive information in our firebase.js file, so let's take care of that.

0.1 in your .gitignore file, add this line:
```jsx
firebase.js
```

0.2 in your terminal, run this command:
```jsx
git rm --chached firebase.js
```
This command ^^ tells git (our version control manager) to stop "tracking" our firebase.js file.

(If we added the firebase.js file to our .gitignore file before ever pushing our code, we wouldn't have to do this second step.)

0.3 then run ```git add``` and ```git commit``` (with a message, obvi), and ```git push```

Okay, now we can get started with the lab!

---

## 1. Add LoginScreen and SignupScreen Components in VSCode

1.1 Make LoginScreen and SignupScreen files / components

In your screens folder, add two files, titled ```LoginScreen.js``` and ```SignupScreen.js```

1.2 Add basic content for LoginScreen and SignupScreen files

<details>
<summary>Copy / Paste this into your LoginScreen.js:</summary>

```jsx
export default function LoginScreen({navigation}) {
	const [email, setEmail] = useState();
	const [password, setPassword] = useState();

	return (
		<>
			<Text style={styles.bigBlue}>Login Here</Text>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Email'
					placeholderTextColor="#003f5c"
					onChangeText={(email) => setEmail(email)}
				/>
			</View>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Password'
					secureTextEntry={true}
					placeholderTextColor="#003f5c"
					onChangeText={(password) => setPassword(password)}
				/>
			</View>
			<TouchableOpacity style={styles.loginBtn} onPress={() => {
				{/* what do you think will go here? */}
			}}>
				<Text style={styles.loginText}>LOGIN</Text>
			</TouchableOpacity>
		</>
	)
}
```
And, these styles:
```jsx
const styles = StyleSheet.create({
	redirectBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"grey",
		color: "white"
	},
	inputView: {
		backgroundColor: "#FFC0CB",
		borderRadius: 30,
		width: "70%",
		height: 45,
		marginBottom: 20,
		alignItems: "center",
	},
	TextInput: {
		height: 50,
		flex: 1,
		padding: 10,
		marginLeft: 20,
	},
	loginBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"#FF1493",
	},
	bigBlue: {
		color: 'blue',
		fontWeight: 'bold',
		fontSize: 30,
		padding: 50
	}
})
```
</details>

<details>
<summary> Copy / Paste into your SignupScreen.js:</summary>

```jsx
import { Text, View, TextInput, StyleSheet, TouchableOpacity } from 'react-native';
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
import {useState} from "react"


export default function LoginScreen({navigation}) {
	const [email, setEmail] = useState();
	const [password, setPassword] = useState();

	return (
		<>
			<Text style={styles.bigBlue}>Signup Here</Text>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Email'
					placeholderTextColor="#003f5c"
					onChangeText={(email) => setEmail(email)}
				/>
			</View>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Password'
					secureTextEntry={true}
					placeholderTextColor="#003f5c"
					onChangeText={(password) => setPassword(password)}
				/>
			</View>
			
			<TouchableOpacity style={styles.loginBtn} onPress={() => {
				{/* what do you think will go here? */}
			}}>
				<Text style={styles.loginText}>Signup</Text>
			</TouchableOpacity>
		</>
	)
}
```
And, these styles:
```jsx
const styles = StyleSheet.create({
	redirectBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"grey",
		color: "white"
	},
	inputView: {
		backgroundColor: "#FFC0CB",
		borderRadius: 30,
		width: "70%",
		height: 45,
		marginBottom: 20,
		alignItems: "center",
	},
	TextInput: {
		height: 50,
		flex: 1,
		padding: 10,
		marginLeft: 20,
	},
	loginBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"#FF1493",
	},
	bigBlue: {
		color: 'blue',
		fontWeight: 'bold',
		fontSize: 30,
		padding: 50
	}
})
```
</details>

---

## 2. Update your App.js file to include our new components

<details>
<summary>Copy / Paste into your App.js file:</summary>

```jsx
import React from "react";
import { StyleSheet } from "react-native";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";

import ChatScreen from "./screens/ChatScreen";
import HomeScreen from "./screens/HomeScreen";
import LoginScreen from "./screens/LoginScreen";
import SignupScreen from "./screens/SignupScreen"

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Chat" component={ChatScreen} />
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Signup" component={SignupScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```
</details>

---

## 3. Configure Firebase Console

3.1 In your Firebase console, select your project, and click "authentication"

(this is the project overview screen)
![https://i.imgur.com/IxA3ikE.png](https://i.imgur.com/IxA3ikE.png)

3.2 Click "get started"

3.3 On the Sign-in method tab in Authentication

- select Email/Password (under Native providers)
- toggle to enable
- save

Your Authentication page should look like this now:
![https://i.imgur.com/a7FKJSK.png](https://i.imgur.com/a7FKJSK.png)

---

## 4. Explore Docs! (for about 5 mins)

4.1 Famialarize yourself with these docs, meaning "skim" through, read the headers, and understand high-level what they are communicating.

https://firebase.google.com/docs/auth/web/password-auth

https://firebase.google.com/docs/auth/web/manage-users#get_the_currently_signed-in_user (stop at "get a user's profile, just go through the first section "Get the currently signed-in user")

---

## 5. Impliment Signup Functionality

5.1 Add in our Signup code, almost exactly as seen in [these docs](https://firebase.google.com/docs/auth/web/password-auth#create_a_password-based_account)

<details>
<summary> Copy / paste into your SignupScreen.js:</summary>

```jsx
import { Text, View, TextInput, StyleSheet, TouchableOpacity } from 'react-native';
import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
import {useState} from "react"

export default function LoginScreen({navigation}) {
	const [email, setEmail] = useState();
	const [password, setPassword] = useState();

	const auth = getAuth();

	async function handleSubmit() {
		console.log("handle submit envoked!!")

		await createUserWithEmailAndPassword(auth, email, password)
		.then((userCredential) => {
			const user = userCredential.user;
			auth.currentUser = user;
		})
		.catch((error) => {
			const errorCode = error.code;
			const errorMessage = error.message;
		});

		navigation.navigate("Home")
	}

	return (
		<>
			<Text style={styles.bigBlue}>Signup Here</Text>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Email'
					placeholderTextColor="#003f5c"
					onChangeText={(email) => setEmail(email)}
				/>
			</View>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Password'
					secureTextEntry={true}
					placeholderTextColor="#003f5c"
					onChangeText={(password) => setPassword(password)}
				/>
			</View>
			<TouchableOpacity style={styles.loginBtn} onPress={() => {
				handleSubmit();
			}}>
				<Text style={styles.loginText}>Signup</Text>
			</TouchableOpacity>
		</>
	)
}
```

You'll also need to paste in these styles:
```jsx
const styles = StyleSheet.create({
	redirectBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"grey",
		color: "white"
	},
	inputView: {
		backgroundColor: "#FFC0CB",
		borderRadius: 30,
		width: "70%",
		height: 45,
		marginBottom: 20,
		alignItems: "center",
	},
	TextInput: {
		height: 50,
		flex: 1,
		padding: 10,
		marginLeft: 20,
	},
	loginBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"#FF1493",
	},
	bigBlue: {
		color: 'blue',
		fontWeight: 'bold',
		fontSize: 30,
		padding: 50
	}
})
```
</details>

5.2 Write 5 comments

Write a minimum of 5 comments in this new file, explaining what a line of code does or asking a question about the code to discuss later. Your comment goes on the line above the code you are referencing. 

Think specific, NOT things like "what does this do" but more like "what does navigation.navigate() do?", or "This sets state, so that the value of password equal to the user's input"


5.3 Notice anything strange about the .catch() section of this function?

<details>
<summary>Hint:</summary>

Pay attention to the color of the variables errorCode and errorMessage
</details>
<details>
<summary>Answer:</summary>

The variables errorCode and errorMessage aren't being used! For our pusposes, let's log them for when we get errors while developing.

Directly below the line ```const errorMessage = error.message```, add:

```jsx
console.log(errorCode, "<---- error code");
console.log(errorMessage, "<--- error message")
```
</details>

5.4 Think about the UX (user experience)

Put yourself in the user's shoes. When signing up for an account, what other screen / page may you want to link to?

<details>
<summary>Hint:</summary>

Think of the only other way to get "into" the app, not signing up
</details>
<details>
<summary>Answer:</summary>

Login! Let's add a button that links to login, in case the user accidently found themselves on the Signup page but already has an account.

Add this code block, at the bottom of your render function:
```jsx
<TouchableOpacity style={styles.redirectBtn} onPress={() => {
		navigation.navigate("Login") 
	}}>
	<Text>Already have an account? Login here</Text>
</TouchableOpacity>
```
</details>

---

## 6. Impliment Login Functionality

6.1 Add in our Login code, almost exactly as seen in [these docs](https://firebase.google.com/docs/auth/web/password-auth#sign_in_a_user_with_an_email_address_and_password)

<details>
<summary>Copy / Paste into your LoginScreen.js: </summary>

```jsx
export default function LoginScreen({navigation}) {
	const [email, setEmail] = useState();
	const [password, setPassword] = useState();

	const auth = getAuth();

	async function handleSubmit() {
		console.log("handle submit envoked!!")

		await signInWithEmailAndPassword(auth, email, password)
		.then((userCredential) => {
			const user = userCredential.user; 
		})
		.catch((error) => {
			const errorCode = error.code;
			const errorMessage = error.message;
		});

		let currentUser = auth.currentUser
		navigation.navigate("Home")
		return currentUser;
	}

	return (
		<>
			<Text style={styles.bigBlue}>Login Here</Text>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Email'
					placeholderTextColor="#003f5c"
					onChangeText={(email) => setEmail(email)}
				/>
			</View>
			<View style={styles.inputView}>
				<TextInput
					placeholder='Password'
					secureTextEntry={true}
					placeholderTextColor="#003f5c"
					onChangeText={(password) => setPassword(password)}
				/>
			</View>
			<TouchableOpacity style={styles.loginBtn} onPress={() => {
				handleSubmit();
			}}>
				<Text style={styles.loginText}>LOGIN</Text>
			</TouchableOpacity>
		</>
	)
}
```
And, these styles:
```jsx
const styles = StyleSheet.create({
	redirectBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"grey",
		color: "white"
	},
	inputView: {
		backgroundColor: "#FFC0CB",
		borderRadius: 30,
		width: "70%",
		height: 45,
		marginBottom: 20,
		alignItems: "center",
	},
	TextInput: {
		height: 50,
		flex: 1,
		padding: 10,
		marginLeft: 20,
	},
	loginBtn: {
		width:"80%",
		borderRadius:25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		marginTop:40,
		backgroundColor:"#FF1493",
	},
	bigBlue: {
		color: 'blue',
		fontWeight: 'bold',
		fontSize: 30,
		padding: 50
	}
})
```
</details>

6.2 Write 5 comments

Write a minimum of 5 comments in this new file, explaining what a line of code does or asking a question about the code to discuss later. Your comment goes on the line above the code you are referencing. 

6.3 Think about the UX, what is this page missing?

<details>
<summary>Hint:</summary>

It is almost identical to a step we did earlier on the SignupScreen component
</details>
<details>
<summary>Answer:</summary>

Add a link to Signup, in case the user does not already have an account registered!

It can look like this (the working doesn't have to be the exact same):
```jsx
<TouchableOpacity style={styles.redirectBtn} onPress={() => {
	navigation.navigate("Signup")
}}>
	<Text>Don't have an account? Sign up here</Text>
</TouchableOpacity>
```
</details>

6.4 Make use of those variables errorCode and errorMessage

Okay, by now you should be catching on for this one. Refer to section 4.3 if need be.

---

## 7. Update HomeScreen.js

7.1 We are almost there! Before we test our code, let's make some "protected routes" and edit our HomeScreen.js file. From [these docs](https://firebase.google.com/docs/auth/web/manage-users#get_the_currently_signed-in_user)

Protected routes are routes (like "Home", "Chat", etc) that are only accessible if there is a user. We are "protecting" the Chat route, by only showing it when a user is logged in. If NO user is logged in, the only routes they can see are "Login" and "Signup".

So, the idea is:
- if you see the "Chat" button, and can view our Chat feature, you are logged in
- if you can only see "Login" and "Signup" buttons, you are logged out

<details>
<summary> Copy / Paste this code into HomeScreen.js:</summary>

```jsx
import React, { useState, useEffect } from "react";
import { Text, View, TouchableOpacity, StyleSheet } from "react-native";
import { getAuth, signOut } from "firebase/auth";


export default function HomeScreen({ navigation }) {

	const [state, setState] = useState();

	const auth = getAuth();
	const user = auth.currentUser;

	useEffect(() => {
		console.log(user, "<-- curr user in home screen")
		setState(user);
	});

	if (user !== null) {
		return (
			<View style={styles.container}>

			<TouchableOpacity style={styles.logoutBtn} onPress={() => {
				signOut(auth).then(() => {
					// Sign-out successful.
					user = null;
				}).catch((error) => {
					// An error happened.
					// should we do something with that error??
				});
				console.log(auth, "<---- auth")
				navigation.navigate("Login")
			}}>
				<Text style={styles.loginText}>sign out</Text>
			</TouchableOpacity>

			<TouchableOpacity
				onPress={() => navigation.navigate("Chat")}
			>
				<Text style={styles.item}>Chat</Text>
			</TouchableOpacity>
			</View>
			)
	} else if (user === null) {
		return (
		<View style={styles.container}>
		<TouchableOpacity
			onPress={() => navigation.navigate("Login")}
		>
			<Text style={styles.item}>login</Text>
		</TouchableOpacity>

		<TouchableOpacity
			onPress={() => navigation.navigate("Signup")}
		>
			<Text style={styles.item}>signup</Text>
		</TouchableOpacity>

			
		</View>
		);
	}
	
}
```
And, these styles:
```jsx
const styles = StyleSheet.create({
	logoutBtn: {
		width:"50%",
		borderRadius:25,
		margin: 25,
		height:50,
		alignItems:"center",
		justifyContent:"center",
		backgroundColor:"grey",
		color: "white"
	},
	container: {
		flex: 1,
		backgroundColor: "#fff",
	},
	item: {
		padding: 10,
		fontSize: 18,
		height: 44,
		backgroundColor: "yellow",
		borderRadius: 25,
		margin: 20
	},
});
```
</details>

7.2 Match our code to [these docs](https://firebase.google.com/docs/auth/web/manage-users#get_the_currently_signed-in_user)

Write 2 comments explaing our "protected routes" code in your HomeScreen.js file

---

## 8. Test your code

8.1 You made it! Let's try to signup, and login to our app

<details>
<summary>note about making "dummy" login accounts</summary>

Since we are using email / password Authentication, we can use anything, even if it is not a real email or domain. "dummy" accounts can look like:
email: demo@demo.com
pw: demodemo

Since we don't have access to these accounts (because they don't exist), I STRONGLY recommend re-using the email and password like so.
</details>

---

You will know that you signed up successfully, because you are automatically logged in after submitting your signup form. How do you know if you are signed in or not? Protected routes! After a successful signup and login you will see the "Chat" button.

In the Firebase console, in Authentication, your users should appear like this:
![https://i.imgur.com/JcW9a4t.png](https://i.imgur.com/JcW9a4t.png)

8.2 Try logging out, signing up a new user, logging in!


---

## Finished Early? 

## Explore: 
Convert either of the handleSubmit() functions from an async to a regular function. Does it function the same / still work? Explain why / why not in a comment in your code.

## Style:
You will be using this code for reference for your final showcase proj, so take some time to style it to look as similar as possible to Chapsnat!

## Challenge: get this to work!

Thinking about the UI (user interface), there are many apps that have a greeting specific to the user near the top nav bar. Let's try to impliment something saying "Hello, user!" (we will have to use user.email beacuse we are not gathering names, but you get the idea).

Try adding something like this to your HomeScreen.js:
```jsx
<Text>Hello, {user.email}! </Text>
```

The user value is not properly assigned at the moment. Your task is to make this <Text> component properly render the current logged in user's email.
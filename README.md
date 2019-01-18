## I will run down the 10 easy steps to setup a React web-application project using React, Typescript, and Webpack. ##

### 1. First, we need is a folder for the project. 

```mkdir react-typescript
cd react-typescript && npm init -y
```

### 2. Now we will install the necessary packages from NPM,

```
npm i react react-dom
```

### 3. The next packages are only development dependencies, the code will be compiled to es5 so we don’t need typescript, the types for the packages or webpack as a dependency in our production code.

```
npm i -D awesome-typescript-loader @types/react @types/react-dom html-webpack-plugin @types/html-webpack-plugin typescript webpack webpack-cli @types/webpack webpack-dev-server ts-node
```

### 4. Now it is time to create some files and folders in our project.

```
mkdir src && cd src && touch index.tsx && touch index.html && mkdir components && cd components && touch app.tsx && cd ../..
```
(with bash script the ampersand’s will let you concatenate commands in the terminal)

### 5. Next, we will create two webpack config files one for the development environment and the other for the production environment.

So create a file in your root folder called webpack.dev.ts with the below code,

```
import * as webpack from "webpack";
import * as HtmlWebPackPlugin from "html-webpack-plugin";

const htmlPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html"
});

const config: webpack.Configuration = {
  mode: "development",
  entry: "./src/index.tsx",
  resolve: {
    // Add '.ts' and '.tsx' as resolvable extensions.
    extensions: [".ts", ".tsx", ".js", ".json"]
  },

  module: {
    rules: [
      // All files with a '.ts' or '.tsx' extension will be handled by 'awesome-typescript-loader'.
      { test: /\.tsx?$/, loader: "awesome-typescript-loader" }
    ]
  },
  plugins: [htmlPlugin]
};
```

Next, we will create the webpack.prod.ts file, it will also live in the root of the project having the below code,

```
import * as path from "path";
import * as webpack from "webpack";
import * as HtmlWebPackPlugin from "html-webpack-plugin";

const htmlPlugin = new HtmlWebPackPlugin({
  template: "./src/index.html",
  filename: "./index.html"
});

const config: webpack.Configuration = {
  mode: "production",
  entry: "./src/index.tsx",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "bundle.js"
  },
  resolve: {
    // Add '.ts' and '.tsx' as resolvable extensions.
    extensions: [".ts", ".tsx", ".js", ".json"]
  },

  module: {
    rules: [
      // All files with a '.ts' or '.tsx' extension will be handled by 'awesome-typescript-loader'.
      { test: /\.tsx?$/, loader: "awesome-typescript-loader" }
    ]
  },
  plugins: [htmlPlugin]
};
export default config;
```

### 6. In the package.json file add the following lines.

```
"scripts": {
		"dev": "webpack-dev-server --open --config webpack.dev.ts",
		"build": "webpack --mode production --config webpack.prod.ts"
	}
```	
  
These lines will add new scripts we use to either run the development server or build a production version of our code.

### 7. Next, we will need to setup compiler options for the typescript configurations this we do in order to be able to run the code we write the way we want it to work.

tsconfig.json file which has to be created in the root folder of the project and having below code,

```
{
	"compilerOptions": {
		"outDir": "./dist/",
		"noImplicitAny": true,
		"target": "es5",
		"lib": ["es5", "es6", "dom"],
		"jsx": "react",
		"allowJs": true,
		"moduleResolution": "node"
	}
}
```

### 8. In the src folder, we added an index.html file. All the HTML code we need is:

```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<meta http-equiv="X-UA-Compatible" content="ie=edge" />
		<title>React Typescript from Scratch</title>
	</head>
	<body>
		<section id="root"></section>
	</body>
</html>
```

### 7. In the components folder we have created an App.tsx file we will have just have a basic functional component which renders an h1 element with the text Hello React Typescript!.

```
import * as React from "react";

export interface IAppProps {}

export default function IApp(props: IAppProps) {
  return <h1>Hello React Typescript!</h1>;
}
```

### 8.In the index.tsx in the src folder we will add the following code:

```
import * as React from "react";
import * as ReactDOM from "react-dom";
import App from "./components/App";
const Index = () => {
  return <App />;
};
ReactDOM.render(<Index />, document.getElementById("root"));
```

### 9. Now fire up your development server and a browser tab should be popping up on port 8080 with the script
```
npm run dev
```

### 10. To add a production build run

```
npm run build
```

This will create a dist folder inside your root folder with an index.html and bundle.js file.

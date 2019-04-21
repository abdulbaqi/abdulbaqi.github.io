---
layout: post
title: Configuring Nginx
subtitle: to run static, node and react contents
bigimg: /img/coding.jpg
image: /img/code-square.jpg
share-img: /img/code-square.jpg
tags: technology react javascript quran
---
This react app explores basics of react app creation and its purpose is to be a tutorial documenting the typical steps. All codes are available in this [github](https://github.com/abdulbaqi/verse-search) repo. 

You can test the finished app [here](http://playquran.com/search/).

First, we create the scaffolding using `npx` as follows:

```
npx create-react-app verse-search
```

### building the basic scaffolding

Next, I will delete all the contents inside the `src` folder and intead create `index.js` file with the following contents.

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './component/App';

ReactDOM.render(<App />, document.querySelector('#root'));
```

Then, we create a folder under `src` called `component` and create `App.js` file with the following content.

```javascript
import React from "react";

class App extends React.Component {
  render() {
    return <div>React started successfully...</div>;
  }
}

export default App;
```

After that we can start the app by `yarn start` (or `npm start`), and head over to

http://localhost:3000/

### Semantic UI

I am going to use **Semantic UI** as styling engine. For that I search cdn site and get it from the following link and insert it in the `public/index.html` file.

https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.4.1/semantic.min.css

### Search Verse Component

The plan is as follows. We will create a Stateful component called `SearchVerse` which will contain input form for searching verses by a key word. The results array of verses that has that keywords is returned to the parent `App` which will then send them as `props` to another child that will `DisplayVerse`. The verses will be searched through an API.

Given this understanding let us first create our `SearchVerse` structure as follows.

```javascript
import React from "react";

class SearchVerse extends React.Component {
  render() {
    return (
      <div className="ui segment">
        <form className="ui form">
          <div className="field">
            <label>Enter search term:</label>
            <input type="text" />
          </div>
        </form>
      </div>
    );
  }
}

export default SearchVerse;
```

Also, a little bit of Semantic UI in the `App` file as follows:

```javascript
class App extends React.Component {
  render() {
    return (
      <div className="ui container" style={{ marginTop: "10px" }}>
        <SearchVerse />
      </div>
    );
  }
}
```

### State system and passing information back and forth

Now comes the tricky parts. First, we will create a state inside `SearchVerse` that would hold the value of `input` form as the user types some term inside the search bar. This is achieved by the followng code which will just display whatever the user types:

```javascript
//insie the SearchVerse.js file

class SearchVerse extends React.Component {
  state = { term: "" };

  render() {
    return (
      <div className="ui segment">
        <form className="ui form">
          <div className="field">
            <label>Enter search term:</label>
            <input
              type="text"
              onChange={e => {
                this.setState({ term: e.target.value });
              }}
            />
          </div>
        </form>
        <p>You are searching for: {this.state.term}</p>
      </div>
    );
  }
}
```

Next, what happens after the `submit` which occurs after pressing `enter`? We want two things to happen

1. first, we do NOT want to refresh the page by trying to send the form over server, that means we want to prevent default i.e., `event.preventDefault()`.
2. We want somehow to pass the search term to the parent, i.e., `App.js`. How to do that?

For that, two steps are needed:
a. In the `App` pass a prop which is a function to the child as follows:

```javascript
class App extends React.Component {
  onFormSubmit = term => {
    console.log(`we are searching for ${term}`);
  };
  render() {
    return (
      <div className="ui container" style={{ marginTop: "10px" }}>
        <SearchVerse onSubmit={this.onFormSubmit} />
      </div>
    );
  }
}
```

b. in the child we implement the handler function and pass the internal state (which we already populated at the time of input search `onChange` handler) to the props called `onSubmit` in the `App`, as follows:

```javascript
class SearchVerse extends React.Component {
  state = { term: "" };
  onFormSubmit = e => {
    e.preventDefault();
    this.props.onSubmit(this.state.term);
  };

  render() {
    return (
      <div className="ui segment">
        <form onSubmit={this.onFormSubmit} className="ui form">
          <div className="field">
            <label>Enter search term:</label>
            <input
              type="text"
              onChange={e => {
                this.setState({ term: e.target.value });
              }}
            />
          </div>
        </form>
        <p>You are searching for: {this.state.term}</p>
      </div>
    );
  }
}
```

### API

We will install `axios` to work with getting API request

```
yarn add axios
```

Next, we need to study the documentation of our API provider, which is available at:
https://alquran.cloud/api

Inside the `App` we need to make an `async` and `await` construct with `axios.get` as follows. Note that our extraction of data from response is highly depended on the nature of API response and could vary from a provider to another.

So, in our case, a typical resutls would be like the following:

```json
{
  "code": 200,
  "status": "OK",
  "data": {
    "count": 72,
    "matches": [
      {
        "number": 131,
        "text": "And [mention, O Muhammad], when Abraham was tried by his Lord with commands and he fulfilled them. [Allah] said, \"Indeed, I will make you a leader for the people.\" [Abraham] said, \"And of my descendants?\" [Allah] said, \"My covenant does not include the wrongdoers.\"",
        "edition": {
          "identifier": "en.sahih",
          "language": "en",
          "name": "Saheeh International",
          "englishName": "Saheeh International",
          "type": "translation"
        },
        "surah": {
          "number": 2,
          "name": "سورة البقرة",
          "englishName": "Al-Baqara",
          "englishNameTranslation": "The Cow",
          "revelationType": "Medinan"
        },
        "numberInSurah": 124
      }
      //next record
    ]
  }

```
Here is the code that uses axios to fetch data asynchroneously.

```javascript
onFormSubmit = async term => {
  const url = `http://api.alquran.cloud/v1/search/${term}/all/en.sahih`;
  const response = await axios.get(url);
  console.log(response.data.data.matches);
  const tmp = [];
  response.data.data.matches.forEach(d => {
    tmp.push(d.text);
  });
  console.log(tmp);

  this.setState({
    count: response.data.data.count,
    verses: tmp
  });
};
```

Now, we pass the resutls stored in our states to another component for the purpose of displaying `DisplayVerses`.

Next, you will find ways to handle conditional rendering based on if no search term is found. 

See how we achieve that in the render function of `App`.

```javascript
render() {
    let message = null;
    if (this.state.count === 0) {
      message = (
        <p className="termnotfound">
          No Results for {this.state.term}. Try again.{" "}
        </p>
      );
    } else if (this.state.count > 0) {
      message = (
        <DisplayVerses
          count={this.state.count}
          verses={this.state.verses}
          term={this.state.term}
        />
      );
    } else {
      message = null;
    }

    return (
      <div className="ui container" style={{ marginTop: "10px" }}>
        <SearchVerse onSubmit={this.onFormSubmit} />

        {this.state.loading ? (
          <Spinner text="Wait..I am finding the verses" />
        ) : (
          message
        )}
      </div>
    );
  }
```
Also, above you will notice that I have introduced a component called `Spinner` that gives a nice waiting feature for the convinience of users while the axios is fetching data. Here is the `Spinner` component:

```javascript
import React from "react";

const Spinner = props => {
  return (
    <div className="ui active dimmer">
      <div className="ui text loader">{props.text}</div>
    </div>
  );
};

Spinner.defaultProps = {
  text: "Wait..I am finding verses"
};

export default Spinner;
```
You can expand this with so many other styling aspects, but let me turn to deployment issues.

## Deployment

What is the value of such an app without exposing it to the world. So, I have my DigitalOcean droplet and I have registered my domain name (playquran.com) and served it through `nginx` engine. So, now I want to deploy this react app to the site so it should be available in a location like `playquran.com/search`. This method basically `builds` the project and serves the built folder as static content through nginx router. I descibed this process in an earlier [post](http://abdulbaqi.io/2019/04/20/Nginx-config/) in my site, from which you can take over the rest of the process.

The other way is to keep the app running on a port in the server through a background process engine like `pm2` and create the appropriate nginx construct as described in [this](https://hackernoon.com/start-to-finish-deploying-a-react-app-on-digitalocean-bcfae9e6d01b) article. 


Thanks for your attention. 


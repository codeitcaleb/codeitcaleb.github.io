---
layout: post
title:      "MyAnime: The Final Project (React-Redux)"
date:       2020-03-16 20:17:01 +0000
permalink:  myanime_the_final_project_react-redux
---


I had so many ideas going into my Final Project at Flatiron School. After pushing the boundaries of my previous projects and implementing libraries and technologies, I expected this one to be the "magnum opus" of my projects. My idea was to incorporate pieces/features of my previous projects into my final project. Well... It didn't quite work out that way. 
After looking back on my plans and what I could do I decided to play it safe and build a version of one of the projects I had done previously. Enter MyAnime.

The idea behind this project was to build on top of the first web application that I had built in this program. It seemed fitting that my final project was something that built onto my first. The inspiration was [](http:///myanimelist.net/anime/season/schedule). As an anime fan, I would have different shows that I would watch on the days that they would air. At times this became difficult to keep organized so as a programmer I set out to solve this problem that I had by creating an application that would help me keep track of what I would watch as well as the days that they aired. 


### Rails API

Part of the requirements for this project was to create a Ruby on Rails API as a backend to handle data persistence. I chose to set up my database with PosgreSQL as this was what I was most familiar with. I set my project up in a way that would allow a user to have many anime on their list or calendar. One of the first hurdles that I ran into was due to time constrant. I wanted to allow a user to have their own account but this required user authentication and was not a requirement for this project. As this was something that I felt was important for the flow of my application, I decided to mock this setup and check for the first user and bypass the need of authentication for this project.

After using the Jikan API for my Sinatra Project, I decided to use it again for this application as well. This allowed me to access an unofficial version of the MyAnimeList API and use the anime data. Previously I had used this to seed my own database but this time I decided to use it to post it to my database only when a user has added it to their list. 

### React

As this was the project was for React-Redux module, the main requirement was to create a React application that incorporated Redux as a way to manage state for this project. Having chosen to only add to the backend after the user makes a selection, I set up my dispatch actions to fetch data from the external API to display the data and then POST that show's data to my backend.

I chose to organize my React code into presentational components and container components. My container components all held and maintained their own state. I could then pass data from this components state onto child components to then be displayed. This allowed me to write my presentational components to being written as functional components and remain stateless, thus providing reusability.

### React Router
One of the aspects of React that gave me the most trouble during this project was how I had to go about implementing React Router. I had gotten used to the way the that Ruby on Rails managed routing and implementing these routes manually in React Router provided me a new appreciation for RESTful routing. The way React handles routing through React Router is called "Dynamic Routing". This means that the routing is handled as the app is rendered rather than on configuration/initialization like Static Routing provided in Ruby on Rails, Express and many other web frameworks. Because almost everything in React is considered a component, the dynamic routing is meant to render a component for each route specified. Once I understood this concept, I was able to design my routes following RESTful architure.

```
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Navbar />
      <Switch>
        <Route exact path="/" component={App} />
        <Route exact path="/calendar" component={AnimeCalendarContainer} />
        <Route exact path="/myanime" component={MyAnime} />
        <Route exact path="/myanime/:id" component={MyAnimeShow} />
        <Route exact path="/anime/:mal_id" component={AnimeShow} />
      </Switch>
    </Router>
  </Provider>
);
```

An issue that I did have to overcome also came from my use of React Router. I found that since my routes were responsible for specific components themselves, I needed to figure out how to pass props to the routes that needed them. This proved difficult at first until I found that i could use the browser history provided in React Router to help me redirect to these routes and pass props through the Links that pointed to them. Once a route matched that path it would render the component attached to it.

```
<Link
              to={{
                pathname: `/anime/${anime.mal_id}`,
                state: { anime }
              }}
            >
              {anime.title}
            </Link>
```

### Redux
After going through the this module I've found that I've really enjoyed working with React but once I got to the Redux section I was thrown off. I understood that it allowed a developer to better manage state and provided a single source of truth. Rather than having to pass props from component to component, Redux would allow you to connect to the Redux "store" where the global state is held and select the data from from state that you would need for each specific component.

Redux works though the use of Dispatch and Reducer functions. These are "pure functions" that do nothing but return a value based on their parameters. Dispatch functions take an an action object as an argument which is then used in the Reducer. The Reducer does this through the use of a case statement that takes the action that it receives as its argument then acts on and returns an updated version of state. With Redux, state is meant to be "immutable" so rather than making changes to it directly in our case statements, we instead make a copy of our state object and make update and return the copy instead.

```
export default function animeReducer(state = {
  anime: [], loading: false, }, action
) {
  switch (action.type) {
    case "LOADING_ANIME":
      return { 
        ...state, 
        loading: true 
      };

    case "GET_ALL_ANIME":
      return {
        ...state,
        anime: action.anime,
        loading: false
      };

    default:
      return state;
  }
}
```
>An example from my animeReducer.

After deciding on which parts of my project needed would need Redux and which parts could manage state locally, I decided to only implement Redux in the MyAnime section of the application(The part directly connected to displaying data from my Rails API). Another part of the requirements was to implement Thunk, a middleware used in Redux that you make action creator functions that return a return a function instead of an action object. The function that is returned is a dispatch function with an action that is then used to call the reducer function.

Probably the greatest aspect of the Thunk middleware is the fact that it allows you to use Redux asynchronously. By default Redux performs tasks synchronously so with Thunk you can complete your dispatch actions once the asynchronous operations have been completed. One of the best examples is with fetch requests. Once the response is received and completed, the dispatch request can then be fired off with the returned data.

```
export const getAnime = () => {
  return dispatch => {
    dispatch({ type: "LOADING_ANIME" });
    fetch("http://localhost:3001/api/animes")
      .then(response => response.json())
      .then(data => {
        return dispatch({ type: "GET_ALL_ANIME", anime: data });
      });
  };
};


```
> An example from my animeActions.


### Conclusion
As I've worked through this project I have learned a lot. What started as just a rehash off of a previous project has turned into a newfound love of working in React. Up to this point I had always dreaded JavaScript but as I've continued to dive deeper into the ecosystem that surround this language and working with the React library, I've realized that this is where I want  to place my focus going forward. I want to continue growing my skills in JavaScript & React and learn React Native for cross-platform mobile app development.

I still have plans to continue building out this application along with my many other project ideas. This was meant to be a full scale application and as I continue to build out this application, it will be. There's only so much that you can do in a week after all.😅

NOTE:
As it stand now, a user can search for anime, add it to their list and see a schedule of shows and the day that it airs. The next iteration of this project will include the authentication aspect that allows a user to have their own private acount. I will also be adding a feature that allows a user to be able to write a review for the anime that they have on their list as well as have their own personal calendar off shows that they watch.

Be on the look out for a React Native version of MyAnime as I continue to learn and grow as a software engineer.



## Redux Primer

### February 4, 2017

***Reducers***
Def.  A function that returns a piece of the application's state.

Because our application can have many different pieces of state,
we can have many different reducers.

    Ex) Book App => two pieces in application state:
        1.  List of books
        2.  Currently selected book

        We could have two reducers.  One for producing the list of books,
        the other for producing the currently selected book.

Reducers produce the value of our state.
    
    Ex) Application State - Generated by Reducers:
        {
            books: [ {title:'Harry Potter'}, {title:'JavaScript'} ],
            activeBook : {title:'JavaScript: The Good Parts'}
        } 

Here, we have the Books reducer and the ActiveBook reducer.

Now, let's write a function that produces the value of our state.  

    For the Books reducer, it should return an array of objects, with each object being the title of the book.  And that array should be assigned to the key 'books' of the application state.

***combineReducers()***
This function defines how we create our application state:

        const rootReducer = combineReducers({
            books: BooksReducer
        });

***Containers***
Def.  A container is a special React component that has direct access to the 'state' produced by Redux.  

Recall that React and Redux are separete libraries.  A third library, React/Redux, is needed to bridge the two.

Created a BookList component and promoted it to a container.

How do we choose which component becomes a container?

    1.  BookList - cares about when the list of books changes
    2.  BookDetail - cares when the active book changes

Only the most parent component that uses a particular piece of 'state' needs  to be connected to Redux.

***Implementation of a Container Class***
Sec. 4, Lec. 42

***React/Redux Recap***

1.  Redux constructs the application state.  React provides the views to display that state.

2.  The two libraries are inherently disconnected.  Use React-Redux to connect the two.

3.  Reducer functions generate the application state.
        ex) reducer_books => always returns an array of books; the array contains a list of objects, with each object having a title property.

4.  Added reducer_books to the combineReducers() call inside reducers/index.js.
This rootReducer creates a key called books whose value is the array from reducer_books.

5.  Created a book-list component.  Since it needed to be aware of state, through the list of books, we promoted this component to a container.

6.  Redux generated a state object that contained our list of books.  We then mapped that state as 'props' to the BookList component.  Because our state was updated, the container rendered into the app.

***Changing State***
We use Actions and Action Creators to change state over time.

Most everything in Redux is triggered by an event, directly or indirectly, by the user.

    a.  Direct Events
            Selecting an item from a dropdown
            Hovering over an element
            Submitting a form

    b.  Indirect Events
            AJAX request finishing
            Page finishing loading

Process
    a.  These events can call an **action creator**.  An action creator is a function that returns an action, which is an object:

            function(return {
                // type of action just triggered (required)
                type: BOOK_SELECTED,
                // payload => additional data that describes action
                book: {title: 'Book 2'}
            })

    b. This action object is automatically sent to all reducers in the application.

    c.  Reducers, depending on the action, will return a different piece of state.

            In reducer, will usually use a switch statement to go to different lines depending on the action:

            switch(action.type) {
                case: BOOK_SELECTED:
                    return action.book
                default:
                    // don't care about this action, so do nothing
                    return current state    
            }

            The returned value will be the new value of state.

    d.  The new state is piped into the application state.

    e.  The application state is then pumped into the React application, causing all the containers to re-render


***Consuming Actions in Reducers***
Because we wired BookList as a container, the selectBook action creator will be sent to all reducers.

1.  Create the active book reducer in the reducers directory:

        reducer_active_book.js

2.  All reducers get two arguments, current state and action:

            export default function (state, action) {

            }

    Reducers are only called when an 'action' occurs.
    State argument is not application state, but only the state this reducer is responsible for.


***Conditional Rendering***
When app first boots up, we don't have any existing application state.  It hasn't been defined yet.  Recall that our application state is defined entirely by the reducers.

So, under the hood, Redux sends some 'booting-up' actions through the reducers.

When our reducer_active_book is called, we skip the switch statement and go to the default value of state = "null"

***Reducers and Actions Review***

1.  Redux is in charge of managing application state.  And state is a single, plain JavaScript object.

2.  Component state is completely separate and distinct from application state.

3.  Application state formed by reducers.

4.  Reducers combined together by combineReducers() method.  Each key in combineReducers object, we assign one reducer.  And that reducer is responsible for creating that portion of application state.

5.  Reducers responsible for changing application state over time.  Changes in state occur through actions.

6.  Whenever an action is dispatched, it flows through all reducers.  Depending on the action dispatched, the reducer has the option of changing its portion of the application state.

7.  Action creators are simple functions that return an action.

8.  An action is just a plain JavaScript object.  Actions must define a type and they can optionally include additional data like a payload property.

















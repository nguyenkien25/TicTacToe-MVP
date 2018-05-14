# TicTacToe - MVP

MVP breaks the controller up so that the natural view/activity coupling can occur without tying it to the rest of the “controller” responsibilities.
More on this below, but let’s start again with a common definition of responsibilities as compared to MVC.

## Model

The model is the Data + State + Business logic of our Tic-Tac-Toe application.
It’s the brains of our application so to speak. It is not tied to the view or controller, and because of this, it is reusable in many contexts.

## View

The only change here is that the Activity/Fragment is now considered part of the view.
We stop fighting the natural tendency for them to go hand in hand.
Good practice is to have the Activity implement a view interface so that the presenter has an interface to code to.
This eliminates coupling it to any specific view and allows simple unit testing with a mock implementation of the view.

## Presenter

This is essentially the controller from MVC except that it is not at all tied to the View, just an interface.
This addresses the testability concerns as well as the modularity/flexibility concerns we had with MVC.
*In fact, MVP purists would argue that the presenter should never have any references to any Android APIs or code.*

Looking at the [TicTacToePresenter](https://github.com/nguyenkien25/TicTacToe-MVP/blob/master/app/src/main/java/com/acme/tictactoe/presenter/TicTacToePresenter.java) in more detail below, the first thing you’ll notice is how much simpler and clearer the intent of each action is.
Rather than telling the view how to display something, it just tells it what to display.

To make this work without tying the activity to the presenter we create an interface that the Activity implements.
In a test, we’ll create a mock based on this interface to test interactions with the view from the presenter.

## Evaluation

This is much cleaner.
We can easily unit test the presenter logic because it’s not tied to any Android specific views and APIs and that also allows us to work with any other view as long as the view implements the TicTacToeView interface.

## Presenter Concerns

- *Maintenance* - Presenters, just like Controllers, are prone to collecting additional business logic, sprinkled in, over time. At some point, developers often find themselves with large unwieldy presenters that are difficult to break apart.

## How can we address this? MVVM to the rescue!

Of course, the careful developer can help to prevent this, by diligently guarding against this temptation as the application changes over time.
However, MVVM can help address this by doing less to start.
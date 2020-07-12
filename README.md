 elm-multi-waitable
> A small package like a traffic light

`MultiWaitable` module manages a state updated asynchronously.

## Example
```elm
-- model


type Model 
    = Loading (MultiWaitable.Wait3 Msg User Options Bookmarks)
    | Loaded User Options Bookmarks


init : Model
init =
    Loading <| MultiWaitable.init3 AllFinished



-- update


type Msg
    = AllFinished
    | UserFetched User
    | OptionsFetched Options
    | BookmarksFetched Bookmarks


update : Model -> Msg -> ( Model, Cmd msg )
update model msg =
    case (model, msg) of
        ( Loading waitable, UserFetched user ) ->
            waitable
                |> MultiWaitable.update3Update1 user
                |> Tuple.mapFirst Loading

        ( Loading waitable, UserOptions options ) ->
            waitable
                |> MultiWaitable.update3Update2 options
                |> Tuple.mapFirst Loading

        ( Loading waitable, BookmarksFetched bookmarks ) ->
            waitable
                |> MultiWaitable.update3Update3 bookmarks
                |> Tuple.mapFirst Loading

        ( Loading _, AllFinished user options bookmarks ) ->
            ( Loaded user options bookmarks, Cmd.none )
```

## Lisence
MIT

## Contribution
PRs accepted

## Prior Art
[0ui/elm-task-parallel](https://package.elm-lang.org/packages/0ui/elm-task-parallel/latest)

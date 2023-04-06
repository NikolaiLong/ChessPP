# Actors - Version 1.0

idea -
  abstract actor class
  stores board, state, pieces, color...

idea -
  all actors are given:
    - reference to the board
      ? would it be more efficient for the board to only store a number placeholder for the piece
      would simplify interaction with neural net
    - reference to the pieces
      ? pieces could hold an array of all the locations where that piece exists
      then getting valid moves would only require parsing the array of pieces
      ? maybe not, pieces have to store more than their location alone - 
      to find their valid moves, they have to assess the board - this could be done with numerical comparisons
      - two arrays of pieces black/white, numerical references on the board only - board doesn't use the pieces
        other than for printing

## Player

need -
  user input control

## Machine - Minimax Decision Tree

idea -
  will essentially be a calculator with very simple goals

idea -
  build a tree of possible moves
  needs to compare (equally weighted?) -
    is someone in checkmate
    what pieces present compared - more value in pieces == higher chance of winning
    where the pieces are compared - more territory == higher chance of winning
      - closer to center
      - pawns around king
      - king on back edge / corner (until piece value has dropped)
  heuristics -
    ( !me_checkmate + them_checkmate*infinity) * (
    piece value sum comparison +
    piece territory value sum comparison )

idea -
  different levels of difficulty related to depth of the tree

requires interaction with game module -
  board copy, pieces - valid moves

tree -
  only one board and set of pieces
  tree will store final heuristic calculation

algo -
  minimax run on the tree, winning branch will be next move chosen

## Artificial Intelligence - Neural Network

idea -
  two sets of heuristics, one for white/black
  algorithm must play differently based on the color of the pieces
  will also help simplify training
  these two can play each other

idea -
  64 inputs, 64 outputs
  for each space on the board

requires interaction with the game module -
  network will move random pieces on the board and compare to valid moves
  if the move is invalid - propogate results through neurons to change heuristics and decisions
    inputs pieces as numbers
      what will those numbers be??
    outputs pieces and compared to valid move boards

phase 1 -
  algorithm learns the rules
    correction ~after each move
  is there a way to instill the rules into the algorithm? and give it an objective?
    not sure, thise can all be taught with reinforcement learning

phase 2 -
  algorithm learns how to win
    correction ~after each game

question -
  how many neurons will be required to make all these complex decisions
  Alpha Zero has ~80 layers ~10,000 neurons per layer, fully connected ~8billion weights

will need to be ported to GCP for execution

64 input ---> neural net ---> 64 output

## Artificial Intelligence - Convolusional Neural Network

idea -
  precursor to the neural net, net will then be fed piece inputs in addition to pattern inputs
  will still output all pieces
  convolve on the grid like an 8x8 pix picture
  each convolusion feeds output to neural net, then continues to a different pattern / smaller size

hypothetical -
  human brain has 85bil neurons
  this nn having 1mil neurons would produce extremely intelligent results

64 input ---------------------> >64 input ---> neural net ---> 64 output
          |-> convolusion --|

## Artificial Intelligence - Convolusional Neural Network with Random Forrest Classification

idea -
  beat Alpha Zero, i don't think it uses rf classification
  not really sure how it will be trained...

hypothetical -
  add on to the CNN, run a random forrest over end-game scenarios to help with pattern identificaiton

64 input ---------------------------------------> >>64 input ---> neural net ---> 64 output
          |-> convolusion --|-> rand forrest -|
          |-> rand forrest -|
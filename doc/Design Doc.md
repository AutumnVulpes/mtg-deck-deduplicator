# Volo Deck Checker Design Doc

Doc plan:

1.  Intro
    1.  Description
        1.  Building Volo deck usually involves creating a deck with no duplicate creature types
        2.  This program is designed to automatically check and feedback for duplicate types
        3.  Will save time, eliminate human error and circumvent edited oracle text
    2.  Scope
        1.  Self contained solo project
    3.  Requirements
        1.  Convert input into usable list of card names
        2.  Handle duplicate creature entries in the input
        3.  Pull card info from database given the card name
        4.  Keep track and record instances of duplicate types and offending cards
        5.  Give output in the same form of input
    4.  Non-goals
        1.  Creating a model to assist with typos and/or incomplete entries in the input
        2.  Web interface/front end for users
        3.  Not considering color identity (Hmm maybe can actually do this??)
2.  Background
    1.  Context/Motivating examples
        1.  Besides just automating a tedious part of deck check process
        2.  Helps remove human error for cards with older text and nonadjusted typing (eg. Wurmcoil Engine \[insert 2 images\] showing how it was errata'd to have phyrexian but older prints do not)
3.  Design Details
    1.  Overview of design
        1.  User feeds in input csv of card names
        2.  Program will use mtg api to query and compare list of names to database to assign each creature its types
        3.  Will output a csv of any duplicate types and offending creatures
    2.  Component digram about the code
    3.  Modules
        1.  (A) Input
            1.  Receives csv commander decklist (100 cards)
            2.  Use api to search and cache database
            3.  Will check to see if there are any illegal duplicates (Eg. Not basiclands, Dragon's Approach, etc)
                1.  t:"basic land"
                2.  t:"basic snow land"
                3.  o:”a deck can have any number of cards named ~”
        2.  (B) Database lookup
            1.  With accepted list, will use cards in list to find them in the db
            2.  Lookup all the cards
        3.  (C) Text comparision/Search
            1.  Discard all non creature cards
            2.  Create a dictionary/list of all the creatures and their respective types
        4.  (D) Duplicate comparision and checking
            1.  For each type not already recorded, record it in a dictionary
            2.  For a type already recorded, in a new dictionary along with the offending creature
            3.  After going through all creatures, add the alst creature of each duplicated type to the dupe dict
            4.  Return duplicate types and creature as csv
    4.  Alternatives Considered
        1.  Checking of duplicates
            1.  Could have used hard coded list ass there are less than 20 exclusions, but use type tags and "problem solving card text" quotes ensures consistency for future content releases
        2.  Different API/Database
            1.  Main options are magicthegathering.io, MTGJSON, scryfall
            2.  Not using gathere due to poor maintanence and incorrect info in the db
        3.  Possible different solution approaches?
4.  Testing Plan

Something like [twisted highscores](http://twistedmatrix.com/highscores )

    bzr branch http://twistedmatrix.com/highscores

## Points

- 200 points for submitting a story or defect to version one
- 500 x story-points for merging a pull request
- 1000 x story-points for a review

"story-points" are version one story complexity estimates

## Architecture

### Provides JSON API

This app provides a JSON API that gives the leaderboard for a particular
month. The API can be consumed by apps such as the Gutsy dashboard.

### Data inputs

- When pull request is merged to master:
    - Committers
    - Reviewers
    - Story points
- When story or defect is added to version one:
    - Creater

### Data storage

Data is stored as JSON files

- one folder per project
- one leaderboard file per month per folder

{'startTimestamp': 1234566,
 'endTimestamp': 123456,
 'leaderboard': [
  ['thisjs', 13605],
  ['exarkun', 7796],
  ...],
 'commitsSeen': [
  ['41a212ee83ca127e3c8cf465891ab7216a705f59',
   'http://github.com/defunkt/github/commit/41a212ee83ca127e3c8cf465891ab7216a705f59'],
  ['de8251ff97ee194a289832576287d6f8ad74e3d0',
   'http://github.com/defunkt/github/commit/de8251ff97ee194a289832576287d6f8ad74e3d0'],
  ...],
  'storiesSeen': [
   ['id', 'url'],
   ...]}

### Post receive Github hook

commitsSeen comes from github post receive hooks

    http://help.github.com/post-receive-hooks/
    
ToDo: Can we retrieve the pull request from this response in order to
look up reviewer and the branch, and thus the story points?

storiesSeen could come from a V1 bot account that watches projects and
then receives email to initiate processing.

### Data polling

Poll V1 and Github once a day to check for changes.


nyt-congress-node
=================
[![NPM](https://nodei.co/npm/nyt-congress-node.png?compact=true)](https://nodei.co/npm/nyt-congress-node/)

[![Build Status](http://img.shields.io/travis/omnibus-app/nyt-congress-node.svg?style=flat
)](https://travis-ci.org/omnibus-app/nyt-congress-node)

Node wrapper for NYT Congress API.

[API Docs](http://developer.nytimes.com/docs/read/congress_api)



## Usage

```javascript

  var Congress = require( 'nyt-congress-node' );
  var client = new Congress( API_KEY );

  client.billDetails({
    'bill-id': 'HR2397',
  }).then( function ( res ) {
    console.log( res );
  });

```

This package works in the browser. To generate a version that will provide a `Congress` browser global, go to the project root and run (assuming you have `browserify` installed globally):

```sh
browserify -s Congress ./ > congress-browser.js
```

Fair warning: the standalone, browserified pacakage is pretty darn big.

`nyt-congress-node` is a straight-forward wrapper around the New York Times Congress API. The Times' developer site has comprehensive documentation as well as example results for each query.

Internally, `nyt-congress-node` uses string interpolation on the API endpoints detailed in the [API documentation](http://developer.nytimes.com/docs/read/congress_api). For example, the bill details endpoint has a url structure as follows

```
http://api.nytimes.com/svc/politics/{version}/us/legislative/congress/{congress-number}/bills/{bill-id}[.response-format]?api-key={your-API-key}
```

A valid request needs to fill in this URL with the following parameters: `version`, `congress-number`, `bill-id`, `response-format`, and an `api-key`.

Of these, only `bill-id` is required. The API key must be passed to the constructor and is automatically added to every request. `congress-number` defaults to 113 (the current congress), and `response-format` defaults to JSON.

Each method takes a parameters object, which it "dasherizes" (turns the keys from `camelCase` to `dash-case`), then interpolates these values into the string.

The example at the top

```javascript
  var Congress = require( 'nyt-congress-node' );
  var client = new Congress( 'API_KEY' );

  client.billDetails({
    'bill-id': 'HR2397',
  }).then( function ( res ) {
    console.log( res );
  });
```
will dispatch a request to the following URL:

```
http://api.nytimes.com/svc/politics/v3/us/legislative/congress/113/bills/HR2397.json?api-key=API_KEY
```

**Every method returns a promise**

## Bills

### `.billsRecent()`
**Delegates to** [Recent bills](http://developer.nytimes.com/docs/read/congress_api#h3-bills)
**Parameters:**
congressNumber
chamber
billType

### `.billsByMember()`
**Delegates to** [Bills by member](http://developer.nytimes.com/docs/read/congress_api#h3-bills-by-member)
**Parameters:**
memberId
billType

### `billDetails()`
**Delegates to** [Bill details](http://developer.nytimes.com/docs/read/congress_api#h3-bill-details)
**Parameters:**
congressNumber
billId

### `billSubjects()`
**Delegates to** [Bill subjects, amendments, and related bills](http://developer.nytimes.com/docs/read/congress_api#h3-bill-subjects) _with resource set to "subjects"_
**Parameters:**
congressNumber
billId

### `billAmendments()`
**Delegates to** [Bill subjects, amendments, and related bills](http://developer.nytimes.com/docs/read/congress_api#h3-bill-subjects) _with resource set to "amendment"_
**Parameters:**
congressNumber
billId

### `billRelatedBills()`
**Delegates to** [Bill subjects, amendments, and related bills](http://developer.nytimes.com/docs/read/congress_api#h3-bill-subjects) _with resource set to "related"_
**Parameters:**
congressNumber
billId

### `billCosponsors()`
**Delegates to** [Bill cosponsors](http://developer.nytimes.com/docs/read/congress_api#h3-bill-cosponsors)
**Parameters:**
congressNumber
billId


## Members

### `memberLists()`
**Delegates to** [Member lists](http://developer.nytimes.com/docs/read/congress_api#h3-members)
**Parameters:**
congressNumber
chamber

### `memberBioAndRoles()`
**Delegates to** [Member bio and roles](http://developer.nytimes.com/docs/read/congress_api#h3-member-roles)
**Parameters:**
memberId

### `membersNew()`
**Delegates to** [New members](http://developer.nytimes.com/docs/read/congress_api#h3-new-members)
**Parameters:**
_None_

### `membersCurrentByStateOrDistrict()`
**Delegates to** [Current members by state/district](http://developer.nytimes.com/docs/read/congress_api#h3-current-member)
**Parameters:**
chamber
state
district

### `membersLeavingOffice()`
**Delegates to** [Members leaving office](http://developer.nytimes.com/docs/read/congress_api#h3-members-leaving)
**Parameters:**
congressNumber

### `memberVotePositions()`
**Delegates to** [Member vote positions](http://developer.nytimes.com/docs/read/congress_api#h3-member-positions)
**Parameters:**
memberId

### `memberVoteComparison()`
**Delegates to** [Member vote comparison](http://developer.nytimes.com/docs/read/congress_api#h3-compare-members)
**Parameters:**
memberId1
memberId2
congressNumber
chamber

### `memberCosponsoredBills()`
**Delegates to** [Bills cosponsored by a member](http://developer.nytimes.com/docs/read/congress_api#h3-cosponsor-bills)
**Parameters:**
memberId
cosponsorType

### `memberSponsorshipComparison()`
**Delegates to** [Member cosponsorship comparison](http://developer.nytimes.com/docs/read/congress_api#h3-member-sponsorship-comparison)
**Parameters:**
memberId1
memberId2
congressNumber
chamber

### `memberFloorAppearances()`
**Delegates to** [Member floor appearances](http://developer.nytimes.com/docs/read/congress_api#h3-member-floor-appearances)
**Parameters:**
memberId


## Nominees

### `nomineeLists()`
**Delegates to** [Nominee lists](http://developer.nytimes.com/docs/read/congress_api#h3-nominees)
**Parameters:**
congressNumber
nominationCategory

### `nomineeDetails()`
**Delegates to** [Nominee details](http://developer.nytimes.com/docs/read/congress_api#h3-nominee-details)
**Parameters:**
congressNumber
nomineeId

### `nomineesByState()`
**Delegates to** [Nominees by state](http://developer.nytimes.com/docs/read/congress_api#h3-nominees-by-state)
**Parameters:**
congressNumber
state


## Other

### `statePartyCounts()`
**Delegates to** [State party counts](http://developer.nytimes.com/docs/read/congress_api#h3-state-parties)
**Parameters:**
_None_

### `committeeList()`
**Delegates to** [Committees and committee members](http://developer.nytimes.com/docs/read/congress_api#h3-committees) _but doesn't accept a committee id_
**Parameters:**
congressNumber
chamber

### `committeeRoster()`
**Delegates to** [Committees and committee members](http://developer.nytimes.com/docs/read/congress_api#h3-committees) _but requires a committee id_
**Parameters:**
congressNumber
chamber
committeeId

### `chamberSchedule()`
**Delegates to** [Chamber schedule](http://developer.nytimes.com/docs/read/congress_api#h3-chamber-schedule)
**Parameters:**
chamber


## Votes

### `votesRollCall()`
**Delegates to** [Roll-call votes](http://developer.nytimes.com/docs/read/congress_api#h3-votes)
**Parameters:**
congressNumber
chamber
sessionNumber
rollCallNumber

### `votesByType()`
**Delegates to** [Votes by type](http://developer.nytimes.com/docs/read/congress_api#h3-votes-by-type)
**Parameters:**
congressNumber
chamber
voteType

### `votesByDate()`
**Delegates to** [Votes by date](http://developer.nytimes.com/docs/read/congress_api#h3-votes-by-date)
**Parameters:**
chamber
year
month

### `votesNominations()`
**Delegates to** [nominationVotes](http://developer.nytimes.com/docs/read/congress_api#h3-nom-votes)
**Parameters: **
congressNumber

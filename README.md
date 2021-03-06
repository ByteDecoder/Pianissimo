# [Pianissimo](https://github.com/AtActionPark/Pianissimo)

Set of musical theory helpers for notes, intervals, chords and scales

Can be installed via nmp, or by copying the dist/pianissimo bundle file in your project

#### Create a pianissimo object

If installed via nmp, require Pianissimo first

```javascript
const pianissimo = require("pianissimo");
```

If the script is copied, the pianissimo object will be available globally

#### Examples

```javascript
const pianissimo = require("pianissimo");

let note1 = pianissimo.note("C3");
let note2 = note1.plusInterval("M9");
note2.getName(); // D4

let interval = pianissimo.interval(note2, "F#4");
interval.invert();
interval.getNotesName(); // ['F#4', D5]
interval.getName(); // d6

let note3 = interval.getNotes()[1]; // D5
let scale = note3.toScale("locrian");
scale.getNotesName(); // ['D5,'Eb5','F5','G5','Ab5','Bb5','C6']

//or, chained, for decreased readability
let scale = pianissimo
  .note("C3")
  .plusInterval("m9")
  .toInterval("F#4")
  .invert()
  .getNotes()[1]
  .toScale("locrian")
  .getNotesName();
// ['D5,'Eb5','F5','G5','Ab5','Bb5','C6']

let chord = pianissimo.chord("Cmaj7b9");
chord.transpose("P5");
chord.getNotesName(); // ['G3','Bb3','Db4','Fb4','Ab4']

let chord2 = pianissimo.chord(["C3", "F#3", "G3", "D4"]);
c.findBestName(); // GΔsus4'
```

#### Methods

```javascript
pianissimo.note(name); //returns a note object from a name or midi note number
pianissimo.interval(name, order); //returns an interval object from a name and order (optional)
pianissimo.interval(note1, note2); //returns an interval object from 2 notes
pianissimo.scale(tonic, type, degree); //returns a scale object from a note, a scale name, and a degree (optional)
pianissimo.chord(tonic, name); //returns a chord object from a note, and a chord name
pianissimo.chord(name); //returns a chord object from a chord name
pianissimo.randomNote(); //returns a random note object
pianissimo.randomInterval(); //returns a random interval object
pianissimo.randomScale(tonic); //returns a random scale object. The tonic is optional
// and will be random if not specified
pianissimo.setA4(frequency); //sets the frequency of A4 (default: 440Hz)
```

## Notes

#### Creation:

Notes are object created through the .note(arg) method.
Argument must be a string of type 'C3', 'C#2', 'Db', 'Solb4', 'fax' ... or a midi number

```javascript
let note = pianissimo.note("C#4");
let note2 = pianissimo.note("Solbb2");
let note3 = pianissimo.note("D"); //octave will default to 3
let note4 = pianissimo.note("Gx4");
let note4 = pianissimo.note(127); //midi ranges from 21 ('A0') to 127 ('G9')
```

#### Getters:

```javascript
note.getName(); // 'C#4'
note.getRoot(); // 'C#'
note.getRootName(); // 'C'
note.getAlteration(); // '#'
note.getOctave(); // 4
note.getNotationType(); // letter (can be letter or name)
note.getMidiNumber(); // 61
```

#### Methods:

**.plusInterval(args)** : adds an interval to the note, and returns the resulting note object.
Arguments can be an interval object, an interval name (order will be ascending) or an interval name and order

```javascript
note.plusInterval(interval); //ex: let note2 = note.plusInterval(intervalObject)
note.plusInterval(intervalName); //ex: let note2 = note.plusInterval('P5')
note.plusInterval(intervaName, order); //ex: let note2 = note.plusInterval('P5', 'descending')
//ex: let note2 = note.plusInterval('P5', '-')
```

**.toInterval(note)** : returns a scale object built on the current note.
Arguments can be the name of the scale, and the degree to start on (optional)

```javascript
let note = pianissimo.note("C3");
note.toInterval("G3"); //returns a P5 interval
```

**.toScale(args)** : returns a scale object built on the current note.
Arguments can be the name of the scale, and the degree to start on (optional)

```javascript
note.toScale(type); //ex: let scale = note.toScale('minor')
note.toScale(type, degree); //ex: let scale = note.toScale('locrian',5)
```

## Interval

#### Creation:

Interval are object created through the .interval(arg1,arg2) method.
The first way of creating an interval is by specifying the name and order.
Arg1 is the name of the interval (m3, P5,d18...)
Arg 2 is optional and is the order (ascending, descending, -, +)

```javascript
let interval1 = pianissimo.interval("m3", "descending");
let interval2 = pianissimo.interval("d5"); //order will default to ascending
let interval3 = pianissimo.interval("P8", "-");
```

The second way is by giving 2 notes.
Arg1 and Arg2 are 2 notes objects or note names

```javascript
let interval1 = pianissimo.interval("C3", "G3");
let interval2 = pianissimo.interval(note1, note2);
let interval3 = pianissimo.interval("C3", note2);
let interval3 = pianissimo.interval(note1, "Sol#");
```

Intervals can also be created from a note object, like so:

#### Getters:

```javascript
interval.getName(); // 'm3'
interval.getSemitonest(); // '3'
interval.getOrder(); // 'ascending'
interval.getNumber(); // 3
interval.getQuality(); // m
interval.getQualityText(); // minor
interval.getNumberText(); // third
interval.getNote1(); // undefined, or the note used to create the interval
interval.getNote2(); // undefined, or the note used to create the interval
interval.getNotes(); // returns [note1,note2]
interval.getNotesName(); // returns [note1.getName(),note2.getName()]
interval.getNotesFrequencies(); // returns [note1.getFrequency(),note2.getFrequency()]
```

#### Methods:

**.invert()** : returns the inverted interval. If it was defined with notes,

```javascript
let interval = new interval("m3").invert();
interval.getName(); // M6

let interval2 = new interval("C3", "G3").invert();
interval.getName(); // M6
interval.getNotesName(); //['G3', 'C4']
```

## Scale

#### Creation:

Scales are object created through the .scale(tonic,type,degree) method.
Degree is optional, with default value 1, and used to start a scale on a different degree

```javascript
let scale1 = pianissimo.scale("C3", "minor"); //C minor
let scale2 = pianissimo.scale("C3", "harmonicMinor", 5); //5th mode of the harmonic minor: C phrygian dominant
```

Scales can also be created from a note object:

```javascript
let note = pianissimo.note("C3");
let scale = note.toScale("minor"); // equivalent to pianissimo.scale('C3','minor')
```

#### Getters:

```javascript
scale.getTonic(); // 'C3'
scale.getName(); // 'C minor'
scale.getType(); // 'minor'
scale.getDegree(); // '1'
scale.getNotes(); // will return an array of note objects
scale.getNotesName(); // will return the name of the notes: ['C3','D3','Eb3','D3','G3','Ab3','Bb3','C4']
scale.getNotesFrequencies(); // will return the frequencies of the notes: [130.81, 145.83, 155.56, 174.61, 196, 207.65, 233.08]
```

#### Methods:

**.getChords()** : returns an array of chords built on the scale, along with diatonic function.
Accepts an optional 'number' argument, to specify the number of notes per chord

```javascript
let note = pianissimo.note("C3");
let scale = pianissimo.scale(note, "major");
let chords = scale.getChords(3);
console.log(chords);
//[ Chord { chord: [ 'C3', 'E3', 'G3' ], name: 'C major tonic' },
//Chord { chord: [ 'D3', 'F3', 'A3' ], name: 'C major supertonic' },
//Chord { chord: [ 'E3', 'G3', 'B3' ], name: 'C major mediant' },
//Chord { chord: [ 'F3', 'A3', 'C3' ], name: 'C major subdominant' },
//Chord { chord: [ 'G3', 'B3', 'D4' ], name: 'C major dominant' },
//Chord { chord: [ 'A3', 'C3', 'E4' ], name: 'C major submediant' },
//Chord { chord: [ 'B3', 'D4', 'F4' ], name: 'C major leadingNote' } ]
```

## Chord

#### Creation:

Chords are object usually created through the .chord(tonic,name) method.
The chord constructor will try to parse the name to build the chord on the tonic.
It should understand most of the usual symols (M,m,Minor,m7,ø,11,add, sus,+,o,dim,aug,...)

```javascript
let chord1 = pianissimo.chord("C3", "minor"); //C minor
let chord2 = pianissimo.chord("C3", "ø"); //C half-diminished
let chord2 = pianissimo.chord("C3", "m(b9b5b7b11)sus4"); //why not
```

Or by supplying only a full chord name

```javascript
let chord = pianissimo.chord("Sol#7b9"); //['Sol#3', 'Si#3' ,'Re#4', 'Fa##4', 'La4']
```

If thats the case, the octave can not be included in the name, and will be 3 by default

Ahords can also be created from a note object:

```javascript
let note = pianissimo.note("C3");
let chord = note.toChord("minor"); // equivalent to pianissimo.chord('C3','minor')
```

Alternatively, chords can also be created by supplying an array of notes (and a optional name)

```javascript
let chord = pianissimo.chord(["C3", "F#4", "Bbb4"], "custom chord");
```

If no name is supplied, the chord creator will naively try to guess the name of the chord and set it

```javascript
let chord = pianissimo.chord(["C3", "Eb3", "Gb3", "Bb3"]); // name: Cdim7
```

#### Getters:

```javascript
chord.getTonic(); // 'C3'
chord.getSymbols(); // 'minor'
chord.getName(); // 'Cminor'
chord.getNotes(); // will return an array of note objects
chord.getNotesName(); // will return the name of the notes: ['C3','Eb3','Bb3']
chord.getNotesFrequencies(); // will return the frequencies of the notes: [130.8, 155.56, 233.08]
```

#### Methods:

**.transpose()** : adds an interval to the chord, and returns the resulting chord object.
Arguments can be an interval object, an interval name (order will be ascending) or an interval name and order

```javascript
let chord = pianissimo.chord("C7b9");
chord.transpose("P5");
console.log(chords.getNotesName());
//[ 'G3', 'B3', 'D4', 'F4', 'Ab4' ]
```

**.invert(order)** : will return an inverted version of the chord. Order will specify the number of inversions. Will default to 1 if no order is specified.

```javascript
let chord = pianissimo.chord("C7");
chord.invert(1);
console.log(chords.getNotesName());
//[ 'E3', 'G3', 'Bb3', 'C4' ]
```

**.findAlternateNames()** : will return a list of possible names for the chord, along with notes order and intervals

```javascript
let chord = pianissimo.chord("C7");
console.log(chords.findAlternateNames());
//[ 'C7 - Notes: C3,E3,G3,Bb3 - Intervals: P1,M3,P5,m7',
//'Edimadd♭6 - Notes: E3,G3,Bb3,C4 - Intervals: P1,m3,d5,m6',
//'Gm6no5add4 - Notes: G3,Bb3,C4,E4 - Intervals: P1,m3,P4,M6',
//'B6sus2no5add♯4 - Notes: Bb3,C4,E4,G4 - Intervals: P1,M2,A4,M6',
//'Edim/C - Notes: C3,E3,G3,Bb3 - Intervals: P1,m3,d5',
//'Gm6no5/C - Notes: C3,G3,Bb3,E4 - Intervals: P1,m3,M6',
//'cant find name - Notes: C3,Bb3,E4,G4 - Intervals: P1,A4,M6' ]
```

**.findBestName()** : will return the shortest name from the possible nams list and set it.

```javascript
let c = pianissimo.chord(["C3", "F#3", "G3", "D4"]);
console.log(c.findBestName());
// GΔsus4'
```

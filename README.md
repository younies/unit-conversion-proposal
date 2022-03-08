# Unit Conversion Proposal

Proposal the cover all the requirements and design for unit conversion starting from simple unit conversion to advanced locale aware unit conversion.

## Requirements
  - Converting between two compound units in the same category
    - For example:
      - Converting from `meter` to `foot`
      - Converting from `kilometer-per-hour` to `mile-per-hour`
      - Converting from `celsius` to `fahrenheit`
      - Converting between reciprocals:
        -Example:  `miles-per-gallon` to `liter-per-100kilometer`
  - Exposing conversion info include [conversion rate, conversion offset, reciprocal or not] 
    - For example:
      - Conversion Info between `foot` to `meter`
        - Conversion Rate: 0.3048
        - Conversion Offset: 0
        - Reciprocal: false
      - Conversion Info between `celsius` and `fahrenheit`
        - Conversion Rate: 1.8
        - Conversion Offset: 32
        - Reciprocal: false
      - Conversion info between `miles-per-gallon` to `liter-per-kilometer`
        -Conversion Rate: 235.215
        - Conversion Offset: 0
        - Reciprocal: true.
  - Converting from a compound unit to mixed unit
    - For example:
      - `meter` to `foot-and-inch`
      - `kilometer-per-hour` to `mile-per-hour`
  - Locale aware units conversion: Converting a compound unit to the corresponding unit/units for a specific locale and usage
    - For example: 
      - Converting the height of a person
        - en_US: `foot-and-inch`
        - en_CH: `meter`
      - Converting the car speed
        - en_US: `mile-per-hour`
        - en_CH: `kilometer-per-hour`

## Proposed API
### For Single Units Conversion:
```js
const singleUnitsConverter =
         intl.UnitsConverter(
                  sourceSingleUnit: "meter",
                  targetSingleUnit: "foot"
                );

console.log(singleUnitsConverter.convert(1.0));
// output should be: 3.28084
```

### For Complex Units Conversion:
```js
const complextUnitsConverter =
         intl.UnitsConverter(
                 sourceSingleUnit: "meter",
                 targetComplexUnit: "foot-and-inch"
                );

console.log(complextUnitsConverter.convert(1.0));
// output should be: 3.0 feet, 3.37 inches
```

### For Locale Aware Units Conversion:
```js
const localeAwareUnitsConverter =
         intl.UnitsConverter(
                 sourceSingleUnit: "meter",
                 locale: "en_US",
                 usage: "person-height"
                );

console.log(localeAwareUnitsConverter.convert(1.0));
// output should be: 3.0 feet, 3.37 inches
```

## Terminologies

| Terminology    | Illustration                                                                                                                                                                                     |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Base Unit      | A unit without any prefix or power. For example: meter, henry, foot … etc.                                                                                                                       |
| Single Unit    | A base unit that may have  a prefix or power. For example: meter, square-meter, square-centimeter … etc.                                                                                         |
| Compound Unit  | A single unit that can be multiplied or divided by another single unit/units. For example, meter, meter-second, meter-per-second, square-meter-per-square-second, liter-per-100-kilometer … etc. |
| Mixed Unit     | Chain of one or more compound units. For example, meter, meter-and-centimeter, foot-and-inch, meter-per-second-and-centimeter-per-second … etc.                                                  |
| Units Category | Represent what a group of units measure. For example, meter, centimeter, foot, inch … etc. are measuring length. Then, their category is  length.                                                |
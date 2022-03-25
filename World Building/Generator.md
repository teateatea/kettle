```html
<!DOCTYPE html>
<html>
  <head>
    <title>Random Background Generator</title>
    <script type="text/javascript">
// - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
// this is a completely self-contained random generator,
// implemented in HTML and JavaScript.
//
// to create a new random generator, simply copy this file
// and change the content of the gen_data array.
//
// the primary key of the gen_data array must be named 'main'.
// to increase the number of random things generated at a time,
// increase the number of rows of the output textarea.
//
// written and released to the public domain by drow [drow@bin.sh]
// http://creativecommons.org/publicdomain/zero/1.0/

// - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
// json format
// http://en.wikipedia.org/wiki/JSON

  let gen_data = {};

  gen_data['main'] = [
    'A {ancestry}, and wielding {weapon}.'
  ];
  gen_data['gender'] = [
    'man', 'woman'
  ];
  gen_data['ancestry'] = {
    '1': 'Caspian',
    '2': 'Breaker',
    '3-17': 'Torian {gender} from {torian_culture}',
	'18': 'Maroian',
	'19': 'Danarak',
	'20': 'Mur'
  };
  gen_data['culture'] = {
    '1': 'the Caspian Dunes',
    '2': 'the Breaker Plains',
    '3-13': "Tal'Arrar",
	'14-18': 'Millburn',
	'19': 'Rathcairn',
	'20': 'the Danarak Jungles'
  };
  gen_data['torian_culture'] = {
    '1-10': 'Phandalin',
    '11-19': 'Millburn',
    '20-28': "Chesterfeld",
	'29': "Tal'Arrar",
	'30': 'Brynn',
	'20': 'Villane'
  };
  gen_data['maroian_culture'] = [
    '{Phandalin}',
    '{melee_weapon} and a shield',
    'twin blades',
    '{ranged_weapon}'
  ];
  gen_data['weapon'] = [
    '{melee_weapon}',
    '{melee_weapon} and a shield',
    'twin blades',
    '{ranged_weapon}'
  ];
  gen_data['melee_weapon'] = [
    'a battleaxe', 'a mace', 'a spear', 'a sword'
  ];
  gen_data['ranged_weapon'] = [
    '{gender}', '{melee_weapon}'
  ];

// - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
    </script>
  </head>
  <body>
    <p><textarea id="output" cols="100" rows="10" readonly></textarea></p>
    <p><input type="button" value="Generate" onclick="more_random();" /></p>
    <script src="data:text/javascript;base64,Ly8gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtIC0gLSAtCi8vIHJhbmRvbS5qcwovLwovLyB3cml0dGVuIGFuZCByZWxlYXNlZCB0byB0aGUgcHVibGljIGRvbWFpbiBieSBkcm93IDxkcm93QGJpbi5zaD4KLy8gaHR0cDovL2NyZWF0aXZlY29tbW9ucy5vcmcvcHVibGljZG9tYWluL3plcm8vMS4wLwoKJ3VzZSBzdHJpY3QnO2Z1bmN0aW9uIG1vcmVfcmFuZG9tKCl7bGV0IGE9ZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoIm91dHB1dCIpO3ZhciBiPXBhcnNlSW50KGEucm93cyk7MT5iJiYoYj0xKTtiPWdlbmVyYXRlX2xpc3QoIm1haW4iLGIpO2EudmFsdWU9Yi5qb2luKCJcbiIpfWZ1bmN0aW9uIGdlbmVyYXRlX3RleHQoYSl7aWYoYT1nZW5fZGF0YVthXSlpZihhPXNlbGVjdF9mcm9tKGEpKXJldHVybiBleHBhbmRfdG9rZW5zKGEpO3JldHVybiIifWZ1bmN0aW9uIGdlbmVyYXRlX2xpc3QoYSxiKXtsZXQgYz1bXSxkO2ZvcihkPTA7ZDxiO2QrKyljLnB1c2goZ2VuZXJhdGVfdGV4dChhKSk7cmV0dXJuIGN9ZnVuY3Rpb24gc2VsZWN0X2Zyb20oYSl7cmV0dXJuIGEuY29uc3RydWN0b3I9PUFycmF5P3NlbGVjdF9mcm9tX2FycmF5KGEpOnNlbGVjdF9mcm9tX3RhYmxlKGEpfQpmdW5jdGlvbiBzZWxlY3RfZnJvbV9hcnJheShhKXtyZXR1cm4gYVtNYXRoLmZsb29yKE1hdGgucmFuZG9tKCkqYS5sZW5ndGgpXX1mdW5jdGlvbiBzZWxlY3RfZnJvbV90YWJsZShhKXt2YXIgYjtpZihiPXNjYWxlX3RhYmxlKGEpKXtiPU1hdGguZmxvb3IoTWF0aC5yYW5kb20oKSpiKSsxO2xldCBjO2ZvcihjIGluIGEpe2xldCBkPWtleV9yYW5nZShjKTtpZihiPj1kWzBdJiZiPD1kWzFdKXJldHVybiBhW2NdfX1yZXR1cm4iIn1mdW5jdGlvbiBzY2FsZV90YWJsZShhKXtsZXQgYj0wLGM7Zm9yKGMgaW4gYSlhPWtleV9yYW5nZShjKSxhWzFdPmImJihiPWFbMV0pO3JldHVybiBifQpmdW5jdGlvbiBrZXlfcmFuZ2UoYSl7bGV0IGI7cmV0dXJuKGI9LyhcZCspLTAwLy5leGVjKGEpKT9bcGFyc2VJbnQoYlsxXSksMTAwXTooYj0vKFxkKyktKFxkKykvLmV4ZWMoYSkpP1twYXJzZUludChiWzFdKSxwYXJzZUludChiWzJdKV06IjAwIj09YT9bMTAwLDEwMF06W3BhcnNlSW50KGEpLHBhcnNlSW50KGEpXX1mdW5jdGlvbiBleHBhbmRfdG9rZW5zKGEpe2Zvcih2YXIgYjtiPS97KFx3Kyl9Ly5leGVjKGEpOyl7Yj1iWzFdO2xldCBjO2E9KGM9Z2VuZXJhdGVfdGV4dChiKSk/YS5yZXBsYWNlKCJ7IitiKyJ9IixjKTphLnJlcGxhY2UoInsiK2IrIn0iLGIpfXJldHVybiBhfW1vcmVfcmFuZG9tKCk7Cg=="></script>
  </body>
</html>
```
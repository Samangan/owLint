# owLint [![Build Status](https://travis-ci.org/Samangan/owLint.svg?branch=master)](http://travis-ci.org/Samangan/owLint)


owLint is a configurable command line OWL file linter. It utilizes the [OWL API](https://github.com/owlcs/owlapi).

owLint is distributed as a tool through [npm](https://www.npmjs.org/package/owlint).
Install owLint globally via the following command: `npm install -g owlint`.

Usage
=======

owLint will look in the defined directory for a .owlint configuration file and .owl files to lint.

An example .owlint file is seen below

```
{
  "ontology-must-have-version-info" : true,
  "ontology-must-have-dc-title" : true,
  "ontology-must-have-dc-creator" : true,
  "ontology-must-have-only-one-dc-creator" : true,
  "ontology-must-have-only-one-dc-contributor" : true,
  "ontology-must-have-dc-date" : true,
  "entities-must-have-rdfs-comment" : true,
  "iris-and-labels-are-unique" : true,
  "non-root-classes-need-genus-differentiation" : true
}
```
A .owlint file is optional. By default all of the tests are enabled.

owLint can also be passed a single file to lint instead of an entire directory. If there is a .owlint file in the directory containing the passed in file then that .owlint file will be used instead of the default.


The output to owlint -h is shown below:

```
owLint: OWL file linting tool 
This tool takes an optional argument, the path to the file to lint or to the folder containing the owl files in question.
Examples:
   $ owlint                             | Use current directory
   $ owlint owlFileFolder               | Use ./owlFileFolder as the directory
   $ owlint /home/username/somedir      | Use absolute path /home/username/somedir as the directory
   $ owlint someOwlFile.owl             | Only lint someOwlFile.owl (You can also use absolute paths for files)
   $ owlint -h | -help                  | Displays help information
```


List of Options
=====================
## Ontology

### ontology-must-have-version-info
Each owl:Ontology must have an [owl:versionInfo](http://www.w3.org/TR/owl-ref/#versionInfo-def) annotation property.

### ontology-must-have-dc-title
Each owl:Ontology must have a [dc:title](http://dublincore.org/documents/2012/06/14/dcmi-terms/?v=elements#title) annotation property.

### ontology-must-have-dc-creator
### ontology-must-have-only-one-dc-creator
Each dc:creator annotation can only have one name listed in the annotation. It is impossible to perfectly enforce this rule, but this can catch the most common ways people try to place more than one person into a single dc:creator annotation.

This test will fail if the dc:creator annotation contains any of the following: 
* new line, 
* carriage return, 
* tab, 
* vertical tab, 
* /, 
* _, 
* |,
* " and " (Notice the spaces padding the word. This is done to make sure that someone who has the substring and in their name will not make this lint fail). 

## Entities


### entities-must-have-rdfs-comment
All classes, individuals, object properties, data properties, and annotation properties defined within the currently linted IRI namespace must have rdfs:comment annotations.
  

## Classes

### non-root-classes-need-genus-differentiation
All non-root classes (Each owl:class with an rdfs:subclassOf statement)
need to have a statement differentiating between other owl:classes that
are also subclasses of the same parent owl:class. An example genus
differentating statement takes the form:
`rdfs:subClassOf <Object Property> <Quantifier> <named owl:class or anonymous class>`
## General

### iris-and-labels-are-unique
The human readable portion of IRIs and rdfs:labels must be a unique set. 

Example of what would trigger a failure:

```
  <owl:Class rdf:about="#Cajun">
...
    <rdfs:label xml:lang="pt">Cajun</rdfs:label>
...
  </owl:Class>
```

Development
===========

### Compliling
``sbt compile``

### Building and Running
``sbt "run -help"``

### Testing
``sbt test``

### Adding additional linter options
To add your linter function add the function and full text description to the lintTestMappings Map.

The linter function you add must be in the following format:

```
def superCoolLinterTest (ontology: OWLOntology): LintFunctionResult = {/**/}
```

If you add an additional option please add relevant tests.


Screenshots
===========
![screenshot](http://i.imgur.com/AnKPqqU.png)


# gaia-interchange

This repository contains resources to support the AIDA Interchange Format (AIF).  It consists of:

*    a formal representation of the format in terms of an OWL ontology in `interchange-format.ttl`.
     This ontology can be validated using the SHACL constraints file in
     `src/main/resources/edu/isi/gaia/aida_ontology.shacl`.

*    utilities to make it easier to work with this format.  JVM utilities are in
     `src/main/java/edu/isi/gaia/AIFUtils.java`. After installation, these can be used from any
     JVM language by adding a Maven dependency on
      `edu.isi:gaia-interchange-kotlin:1.0.0-SNAPSHOT`.  Next Century will be adding a Python
      translation of these utilities soon.

*    examples of how to use AIF. These are given in the unit tests under
     `src/text/java/edu/isi/gaia/ExamplesAndVlaidationTests`.  Next Century will be translating
     these to Python soon.  If you run the tests via

*    code to translate from the TAC KBP Coldstart++ KB format into this format.  Both Python
     (`gaia-interchange/gaia_interchange/coldstart2gaia.py`) and
     JVM (`src/main/java/edu/isi/gaia/ColdStart2AidaInterchange.kt`; usable from Java)
     versions are provided as examples of how to use the format in code, but the Python version is
     out-of-date.

# Installation

* To install the JVM code, do `mvn install` from the root of this repository using Apache Maven.
* The Python code is not currently set up for installation; just add it to your `PYTHONPATH`.

# Running the validator

To run the validator, run `target/appassembler/bin/validateAIF` with a single argument, a parameter
file. The parameter file should have keys and values separated by `:`. It should have either the
parameter `kbToValidatie` pointing to the single Turtle format KB to validate, or it should have
`kbsToValidate` pointing to a file listing the paths of the Turtle format KBs to validate.

# Running the ColdStart -> AIF Converter

# Kotlin

If you don't have it already, install Apache Maven.  From the root of this repository, run
`mvn install`.   Repeat `mvn install` if you pull an updated version of the code.

To convert a ColdStart KB, run `target/appassembler/bin/coldstart2AidaInterchange`. It takes a
single argument, a key-value parameter file where keys and values are separated by `:`s.  There
are four parameters which are always required:
* `inputKBFile`: the path to the ColdStart KB to convert
* `baseUri`: the URI path to use as the base for generated assertions URIs, entity URIs, etc.  For
    example `http://www.isi.edu/aida`
* `systemUri`: a URI path to identify the system which generated the ColdStart output. For
    example `http://www.rpi.edu/tinkerbell`
* `mode`: must be `FULL` or `SHATTER`, as explained below.

If `mode` is `FULL`, then the entire ColdStartKB is converted into a single AIF RDF file in
n-triples format (n-triples is used for greater I/O speed).  The following parameters then
 apply:
 * `outputAIFFile` will specify the path to write this file to.
 * cross document coreference information present in the ColdStartKB can be discarded by setting
     the optional parameter `breakCrossDocCoref` to `true` (default `false`).
* The optional `restrictConfidencesToJustifications` parameter (default `false`) controls whether
   confidence values are attached directly to the relevant entity/relation/event/sentiment
   assertion or only to their justifications.  The former is how it should be in TA2/TA3, but the
   latter for messages from TA1 to TA2.  Note this is somewhat imperfect because ColdStart
   lacks justifications for type and link assertions, so for these confidence information will
   simply be missing when restricting to justifications.

If `mode` is `SHATTER`, the data related to each document in the ColdStart KB is written to a
separate AIF file in Turtle format.  In this case, the only other parameter is the directory
to write the output to (`outputAIFDirectory`).  The values of `breakCrossDocCoref`,
`useClustersForCoref`, and `attachConfidencesToJustifications` are fixed at `true`, `false`,
and `true`, respectively.

The following optional parameters are available in both modes:
* `useClustersForCoref` parameter (default `false`) specifies whether
      to use the "clusters" provided by the AIF format for representing coreference.  While in AIDA
      there can be uncertainty about coreference, making these clusters useful, in ColdStart
      coreference decisions were always "hard".  We provide the user with the option of whether to
      encode these coref decisions in the way they would be encoded in AIDA if there were any
      uncertainty so that users can test these data structures.


# Python

*    The Python code is not runnable until we release a few internal utilities it uses, which can
     be done as soon as anyone lets us know they want to try it out.
* The Python code writes only to document-shattered Turtle files.

# Legal Stuff

This material is based on research sponsored by DARPA under agreement number FA8750-18- 2-0014.
The U.S. Government is authorized to reproduce and distribute reprints for Governmental purposes
notwithstanding any copyright notation thereon.

The views and conclusions contained herein are those of the authors and should not be interpreted
as necessarily representing the official policies or endorsements, either expressed or implied, of
DARPA or the U.S. Government.

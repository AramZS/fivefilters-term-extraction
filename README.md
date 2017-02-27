Term Extractor
==============

This is a PHP port of the [Topia's Term Extractor](http://pypi.python.org/pypi/topia.termextract/)

It was ported to PHP for the [Five Filters](http://fivefilters.org) project.

It also incorporates some of the [changes made by Joseph Turian](https://github.com/turian/topia.termextract) to Topia's Term Extractor.

It is licensed under the GPL 3.0 and the original license (ZPL 2.1).

We also offer a self-hosted web service using this package which replicates Yahoo's Term Extraction API and offers term extraction for online web articles. If you are looking to run this as a web service, or carry out term extraction on web articles, please see <http://fivefilters.org/term-extraction/>.

### Donate

If you find this useful, please consider purchasing our [Term Extractor web service](http://fivefilters.org/term-extraction/) or donating via [Gittip](https://www.gittip.com/fivefilters/)

### Simple example
	<?php
	// Text to extract terms from
	$text = 'Inevitably, then, corporations do not restrict themselves merely to the arena of economics. Rather, as John Dewey observed, "politics is the shadow cast on society by big business". Over decades, corporations have worked together to ensure that the choices offered by \'representative democracy\' all represent their greed for maximised profits.

	This is a sensitive task. We do not live in a totalitarian society - the public potentially has enormous power to interfere. The goal, then, is to persuade the public that corporate-sponsored political choice is meaningful, that it makes a difference. The task of politicians at all points of the supposed \'spectrum\' is to appear passionately principled while participating in what is essentially a charade.';

	// TermExtractor PHP class (if not using Composer's autoloader)
	require 'term-extractor/TermExtractor.php';

	$extractor = new TermExtractor();
	$terms = $extractor->extract($text);
	// We're outputting results in plain text...
	header('Content-Type: text/plain; charset=UTF-8');
	// Loop through extracted terms and print each term on a new line
	foreach ($terms as $term_info) {
		// index 0: term
		// index 1: number of occurrences in text
		// index 2: word count
		list($term, $occurrence, $word_count) = $term_info;
		echo "$term\n";
	}
	
### Advanced example
	<?php
	// Text to extract terms from
	$text = 'Inevitably, then, corporations do not restrict themselves merely to the arena of economics. Rather, as John Dewey observed, "politics is the shadow cast on society by big business". Over decades, corporations have worked together to ensure that the choices offered by \'representative democracy\' all represent their greed for maximised profits.

	This is a sensitive task. We do not live in a totalitarian society - the public potentially has enormous power to interfere. The goal, then, is to persuade the public that corporate-sponsored political choice is meaningful, that it makes a difference. The task of politicians at all points of the supposed \'spectrum\' is to appear passionately principled while participating in what is essentially a charade.';

	// include PHP files (not needed if using Composer's autoloader)
	require 'term-extractor/TermExtractor.php';
	require 'term-extractor/DefaultFilter.php';
	

	// Filters
	// -------
	// Permissive - accept everything
	//require '../TermExtractor/PermissiveFilter.php';
	//$filter = new PermissiveFilter();

	// Default - accept terms based on occurrence and word count
	// min_occurrence - specify the number of times the term must appear in the original text for it be accepted.
	// keep_if_strength - keep a term if the term's word count is equal to or greater than this, regardless of occurrence.
	$filter = new DefaultFilter($min_occurrence=2, $keep_if_strength=2);

	// Tagger
	// ------
	// Create Tagger instance.
	// English is the only supported language at the moment.
	$tagger = new Tagger('english');
	// Initialise the Tagger instance.
	// Use APC if available to store the dictionary file in memory 
	// (otherwise it gets loaded from disk every time the Tagger is initialised).
	$tagger->initialize($use_apc=true); 

	// Term Extractor
	// --------------
	// Creater TermExtractor instance
	$extractor = new TermExtractor($tagger, $filter);
	// Extract terms from the text
	$terms = $extractor->extract($text);
	// We're outputting results in plain text...
	header('Content-Type: text/plain; charset=UTF-8');
	// Loop through extracted terms and print each term on a new line
	foreach ($terms as $term_info) {
		// index 0: term
		// index 1: number of occurrences in text
		// index 2: word count
		list($term, $occurrence, $word_count) = $term_info;
		echo "$term\n";
		echo "  ->  occurrence: $occurrence, word count: $word_count\n\n";
	}
# chatbot
  def __init__(self, language):  
   def __init__(self, language):
        super().__init__(language)    
        super().__init__(language)
        import spacy        try:
            import spacy
        except ImportError:
            message = (
                'Unable to import "spacy".\n'
                'Please install "spacy" before using the SpacySimilarity comparator:\n'
                'pip3 install "spacy>=2.1,<2.2"'
            )
            raise OptionalDependencyImportError(message)

            self.nlp = spacy.load(self.language.ISO_639_1)   
            self.nlp = spacy.load(self.language.ISO_639_1)
@@ -108,7 +117,15 @@ class JaccardSimilarity(Comparator):
      def __init__(self, language):  
      def __init__(self, language):
        super().__init__(language)   
        super().__init__(language)
        import spacy        try:
            import spacy
        except ImportError:
            message = (
                'Unable to import "spacy".\n'
                'Please install "spacy" before using the JaccardSimilarity comparator:\n'
                'pip3 install "spacy>=2.1,<2.2"'
            )
            raise OptionalDependencyImportError(message)


        self.nlp = spacy.load(self.language.ISO_639_1)   
        self.nlp = spacy.load(self.language.ISO_639_1)


  21 changes: 19 additions & 2 deletions21  
chatterbot/corpus.py
@@ -1,11 +1,18 @@
import os import os
import io import io
import glob import glob
import yaml from pathlib import Path
from chatterbot.exceptions import OptionalDependencyImportError

try: try:
    from chatterbot_corpus.corpus import DATA_DIRECTORY  
    from chatterbot_corpus.corpus import DATA_DIRECTORY
except (ImportError, ModuleNotFoundError):
except (ImportError, ModuleNotFoundError):
    DATA_DIRECTORY = os.path.join(os.path.dirname(os.path.abspath(__file__)), 'data')  
    DATA_DIRECTORY = os.path.join(
        Path.home(),
        'chatterbot_corpus',
        'data'
    )
CORPUS_EXTENSION = 'yml' CORPUS_EXTENSION = 'yml'
@@ -38,6 +45,16 @@ def read_corpus(file_name):
    """    """
    Read and return the data from a corpus json file. 
    Read and return the data from a corpus json file.
    """    """
    try:
        import yaml
    except ImportError:
        message = (
            'Unable to import "yaml".\n'
            'Please install "pyyaml" to enable chatterbot corpus functionality:\n'
            'pip3 install pyyaml'
        )
        raise OptionalDependencyImportError(message)

    with io.open(file_name, encoding='utf-8') as data_file:   
    with io.open(file_name, encoding='utf-8') as data_file:
        return yaml.safe_load(data_file)     
        return yaml.safe_load(data_file)
class OptionalDependencyImportError(ImportError):
    """
    An exception raised when a feature requires an optional dependency to be installed.
    """
    pass
from datetime import datetime from datetime import datetime
from chatterbot.logic import LogicAdapter from chatterbot.logic import LogicAdapter
from chatterbot.conversation import Statement from chatterbot.conversation import Statement
from chatterbot.exceptions import OptionalDependencyImportError
class TimeLogicAdapter(LogicAdapter): class TimeLogicAdapter(LogicAdapter):
@@ -18,7 +19,15 @@ class TimeLogicAdapter(LogicAdapter):
   def __init__(self, chatbot, **kwargs):
   def __init__(self, chatbot, **kwargs):
        super().__init__(chatbot, **kwargs)    
        super().__init__(chatbot, **kwargs)
        from nltk import NaiveBayesClassifier     
        try:
            from nltk import NaiveBayesClassifier
        except ImportError:
            message = (
                'Unable to import "nltk".\n'
                'Please install "nltk" before using the TimeLogicAdapter:\n'
                'pip3 install nltk'
            )
            raise OptionalDependencyImportError(message)


        self.positive = kwargs.get('positive', [      
        self.positive = kwargs.get('positive', [
          'what time is it',            'what time is it',
from chatterbot.logic import LogicAdapter from chatterbot.logic import LogicAdapter
from chatterbot.conversation import Statement from chatterbot.conversation import Statement
from chatterbot.exceptions import OptionalDependencyImportError
from chatterbot import languages from chatterbot import languages
from chatterbot import parsing from chatterbot import parsing
from mathparse import mathparse from mathparse import mathparse
@@ -22,7 +23,15 @@ class UnitConversion(LogicAdapter):
      def __init__(self, chatbot, **kwargs): 
      def __init__(self, chatbot, **kwargs):
        super().__init__(chatbot, **kwargs)    
        super().__init__(chatbot, **kwargs)
        from pint import UnitRegistry     
        try:
            from pint import UnitRegistry
        except ImportError:
            message = (
                'Unable to import "pint".\n'
                'Please install "pint" before using the UnitConversion logic adapter:\n'
                'pip3 install pint'
            )
            raise OptionalDependencyImportError(message)


        self.language = kwargs.get('language', languages.ENG)    
        self.language = kwargs.get('language', languages.ENG)
        self.cache = {}        self.cache = {}
@@ -82,7 +91,7 @@ def get_unit(self, unit_variations):


    def get_valid_units(self, from_unit, target_unit): 
    def get_valid_units(self, from_unit, target_unit):
        """        """
        Returns the firt match `pint.unit.Unit` object for from_unit and     
        Returns the first match `pint.unit.Unit` object for from_unit and
        target_unit strings from a possible variation of metric unit names   
        target_unit strings from a possible variation of metric unit names
        supported by pint library.



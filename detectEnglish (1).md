# Wykryj moduł w języku angielskim
# https://www.nostarch.com/crackingcode/ (licencja BSD)

# Aby użyć, wpisz ten kod:
#  import detectEnglish
#  detectEnglish.isEnglish(someString) #zwraca wartość Prawda lub Fałsz
# (W tym katalogu musi znajdować się plik „dictionary.txt” ze wszystkimi)
# Angielskimi słowami każde słowo w pojedynczej linii. Ypu może je pobrać z
#  https://www.nostarch.com/crackingcodes/.)

UPPERLETTERS = 'ABCDEFGHIJKLMONPQRSTUVWXYZ'
LETTERS_AND_SPACE = UPPERLETTERS + UPPERLETTERS.lower() + ' \t\n'

def loadDictionary():
    dictionaryFile = open('dictionary.txt')
    englishWords = {}
    for word in dictionaryFile.read().split('\n'):
        englishWords[word] = None
    dictionaryFile.close()
    return englishWords

ENGLISH_WORDS = loadDictionary()

def getEnglishCount(message):
    message = message.upper()
    message = removeNonLetters(message)
    possibleWords = message.split()

    if possibleWords == [] :
        return 0.0 # No words at all, so return 0.0

    matches = 0
    for word in possibleWords:
        if word in ENGLISH_WORDS:
            matches += 1
    return float(matches) / len(possibleWords)

def removeNonLetters(message):
    lettersOnly = []
    for symbol in message:
        if symbol in LETTERS_AND_SPACE:
            lettersOnly.append(symbol)
    return ''.join(lettersOnly)

def isEnglish(message, wordPercentage=20, letterPerchentage=85 ):
    # By default, 20% of the words must exist in the dictionary file, and
    # 85% of all the characters in the message must be letters or spaces
    # (not punctuation or numbers).

    wordsMatch = getEnglishCount(message) * 100 >= wordPercentage
    numLetters = len(removeNonLetters(message))
    messageLettersPercentage = float(numLetters) / len(message) * 100
    lettersMatch = messageLettersPercentage >= letterPerchentage
    return wordsMatch and lettersMatch
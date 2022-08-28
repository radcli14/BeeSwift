# BeeSwift
Add Python to a SwiftUI application with the BeeWare Briefcase

## Setup Virtual Environment

```
python3 -m venv venv
source venv/bin/activate
pip install sympy
python -m pip install briefcase
```

## Start a New Project

```
briefcase new
```

Responses ...

```
Formal Name [Hello World]: Bee Swift
App Name [beeswift]:
Bundle Identifier [com.example]:
Project Name [Bee Swift]: 
Description [My first application]: Add Python to a SwiftUI application with the BeeWare Briefcase
Author [Jane Developer]:
Author's Email [jane@example.com]:
Application URL [[https://example.com/beeswift]:
Project License [1]:
GUI Framework [1]:
```

## Set Python Requirements

```
cd beeswift
vim pyproject.toml
```

Type `a` to append text, then scroll to the line that reads `requires = ...` and modify to

```
requires = [
    'toga-cocoa>=0.3.0.dev34',
    'std-nslog~=1.0.0',
    'sympy'
]
```
Press the `esc` button and then type `:wq` and `enter` to write and quit.


## Create the iOS Project

```
briefcase create iOS
```

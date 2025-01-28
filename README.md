# Bygga workflows med GitHub Actions


## 📚 Innehållsförteckning
1. [Introduktion](#introduktion)
2. [Hur fungerar det?](#hur-fungerar-det)
3. [Workflow-fil (YAML)](#workflow-fil-yaml)
4. [Hur man använder workflowet](#hur-man-använder-workflowet)

---

## 📖 Introduktion
När man arbetar med ett projekt i GitHub är det viktigt att validera koden automatiskt för att undvika att felaktig kod hamnar i `main`. Ett **build workflow** säkerställer att:

- Koden kompileras korrekt.
- Alla beroenden är korrekta.
- En fungerande kod upprätthålls i hela projektet.

---


## ⚙️ Hur fungerar det?
Workflowet använder GitHub Actions för att köra en rad steg (steps) varje gång en förändring görs i projektet. 
När workflowet körs så kan man följa det i github. I detta exempel ser det ut såhär:

![image](https://github.com/user-attachments/assets/1d06466d-ebf9-46ae-8987-8a1b3c75626a)

---

## 📝 Workflow-fil (YAML)

I detta exempel används följande YAML-fil som används för att skapa ett enkelt build-workflow. 

```yaml
name: Build Project   # name: namnet på workflow

on:
# on: här definieras de triggers (händelser) som ska starta workflowet
  push:
    branches:
      - main
 # Kör workflowet vid push till main
  pull_request:  
    branches:
      - main
# Kör även workflowet vid pull requests mot main

jobs:
 #    Ett workflow kan innehålla ett eller flera jobs.
 #    Varje job är en sekvens av steg (steps) som körs på en specifik miljö.

  build:   # Detta är namnet på detta job, dvs 'build'
    runs-on: ubuntu-latest

  # runs-on används eftersom en virtuell maskin ska köra jobbet
  # När workflowet startar (t.ex. efter en push till main), tillhandahåller GitHub Actions
  #  en ny virtuell maskin (VM) baserad på det operativsystem du valt
  # (i det här fallet ubuntu-latest).

 # Nedan följer fyra steg som ingår i detta job 
    steps:
    - name: Checkout code
     # steg 1
      uses: actions/checkout@v2  # Klona koden från repository

    - name: Setup .NET
     # steg 2 
      uses: actions/setup-dotnet@v3  # Installera .NET SDK
      with:
        dotnet-version: '8.0'  # Ska vara samma som din egna .NET version 

    - name: Restore dependencies
      # steg 3 
      run: dotnet restore  # Installera NuGet-beroenden

    - name: Build solution
      # steg 4 
      run: dotnet build --configuration Release  # Bygg projektet
```
--- 

### Om kommandot som körs i steg 4 

![image](https://github.com/user-attachments/assets/3429090e-9928-4e99-b396-0c04a3f8614d)

Kommandet ovan används för att bygga ett .NET-projekt (eller en lösning) i Release-läge,
vilket innebär att projektet byggs för produktionssättning eller distribution.

När du använder GitHub Actions för att bygga ditt .NET-projekt, kommer byggresultaten (t.ex. de kompilerade filer som skapas)
att finnas på den virtuella maskinen som GitHub Actions kör. Dock är dessa byggresultat temporära och kommer inte 
att bevaras när byggprocessen är klar om du inte uttryckligen sparar dem.

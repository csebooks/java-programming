# Java Programming


## Dev

```sh
git clone https://github.com/gurukulams/design-system themes/design-system
hugo server  --themesDir ../ --disableFastRender
```

## Qustion Loader

in Linux

```bash
export QUESTIONS_FOLDER="$PWD/questions"
export PUBLIC_FOLDER="$PWD/public" 
npm i --prefix ../design-system
npm run watch --prefix ../design-system
```

in Windows `Cmd`

```bash
set QUESTIONS_FOLDER=%cd%\questions
set PUBLIC_FOLDER=%cd%\public
cd themes\design-system
npm i
npm run watch
```
# React hooks

## Setup

Pour faire simple et rapide, j'ai utilisé l'outil de scaffolding `create-react-app`.

On commence donc par lancer cette commande dans un terminal : `npx create-react-app nom_de_mon_appli`. L'outil s'occupe d'installer tout ce qu'il faut. En cas de problème, voir [leur GitHub](https://github.com/facebook/create-react-app).

On va modifier la version de `react` et de `react-dom` pour bénéficier des hooks, qui ne sont pas inclus dans la version stable de react pour l'instant. Comme on va faire des tests, on va aussi inclure quelques utilitaires. Voici les commandes à lancer dans un terminal : 

```
npm i react@16.7.0-alpha.2 react-dom@16.7.0-alpha.2
npm i --save-dev jest-dom react-test-renderer react-testing-library
```

On peut démarrer la création de nos composants.
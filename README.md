# Découvrir les React Hooks

Lors de la [React Conf 2018](https://www.youtube.com/watch?v=dpw9EHDh2bM), Facebook a présenté une nouveauté de l'équipe pour simplifier l'écriture d'applications. Avec une stack classique React-Redux, pour les composants contenant ou manipulant une partie de l'état de l'application \(stateful components\), on est obligé d'utiliser des classes. Ceci a l'inconvénient d'être plus verbeux qu'un composant fonctionnel, donc plus dur à maintenir ou à refactoriser.



Dan Abramov, spécialiste des [conférences avec effet d'annonces](/blog/gestion-de-l-etat-dans-react-avec-redux-1-5#architecture), présente une solution permettant de gérer l'état de l'application dans des fonctions plutôt que des classes, et même peut-être se passer de Redux, la solution dont il est co-auteur et qui lui a valu son embauche chez Facebook.


# React hooks

## Rendu et création de posts

Commençons par le rendu des posts. Un post est constitué d'un contenu, de tags et d'un auteur. Voici la structure, toute simple, dans un composant fonctionnel :

src/post/RenderPost.js
```js
import React from 'react'
import PropTypes from 'prop-types'

const Post = props => {
  const post = props.post
  return (
    <div className="post">
      <div className="post__content">{post.content}</div>
      <div className="post__tags">{post.tags}</div>
      <div className="post__author">{post.author}</div>
    </div>
  )
}

Post.propTypes = {
  number: PropTypes.number,
  content: PropTypes.string,
  tags: PropTypes.array,
  author: PropTypes.string,
}

export default Post
```

Pour la liste des posts, nous allons faire appel à ce composant `RenderPost` et afficher 2 boutons qui serviront au tri des posts.

src/post/Posts.js
```js
import React from 'react'
import RenderPost from './RenderPost'

const RenderPosts = props => {
  const sortByTag = () => {
    const newPosts = props.posts.sort((a, b) =>
      a.tags[0].trim().localeCompare(b.tags[0].trim()),
    )
    props.sortPosts(newPosts)
  }

  const sortByNumber = () =>
    props.sortPosts(props.posts.sort((a, b) => a.number - b.number))

  return (
    <div className="posts">
      <div className="posts__buttons">
        <button onClick={sortByTag}>Sort by tag</button>&nbsp;
        <button onClick={sortByNumber}>Sort by number</button>
      </div>
      <div className="posts__render">
        {props.posts.map((post, index) => (
          <RenderPost post={post} key={index} />
        ))}
      </div>
    </div>
  )
}

export default RenderPosts
```

Jusque-là, rien de très complexe. La création de post va illustrer la force des hooks.

src/post/CreatePost.js
```js
import React, { useState } from 'react'
import PropTypes from 'prop-types'

const CreatePost = props => {
  const initialState = {
    content: '',
    tags: [],
    author: '',
  }
  const [post, setPost] = useState(initialState)
  const [message, setMessage] = useState('')

  const handleInputChange = event => {
    const { name, value } = event.target
    setMessage('')
    document.getElementById('content').classList.remove('validate-fail')
    setPost({ ...post, [name]: value })
  }

  const handleSubmit = event => {
    event.preventDefault()
    if (!post.content) {
      setMessage('Missing field content.')
      document.getElementById('message').classList.add('message-fail')
      document.getElementById('content').classList.add('validate-fail')
      return
    }

    post.tags = [post.tags]
    props.addPost(post, props.setPosts)
    setPost(initialState)
    setMessage('Post added.')
    document.getElementById('message').classList.add('message-success')
  }

  return (
    <form onSubmit={handleSubmit} className="create-post">
      <div className="create-post__content">
        <label htmlFor="content">Content</label>
        <textarea
          name="content"
          id="content"
          value={post.content}
          onChange={handleInputChange}
        />
      </div>
      <div className="create-post__tags">
        <label htmlFor="tags">Tags</label>
        <input
          type="text"
          name="tags"
          id="tags"
          value={post.tags}
          onChange={handleInputChange}
        />
      </div>
      <div className="create-post__author">
        <label htmlFor="author">Author</label>
        <input
          type="text"
          name="author"
          id="author"
          value={post.author}
          onChange={handleInputChange}
        />
      </div>
      <br />
      <input type="submit" data-testid="submit" value="Submit" />
      <label id="message">{message}</label>
    </form>
  )
}

CreatePost.propTypes = {
  content: PropTypes.string,
  tags: PropTypes.array,
  author: PropTypes.string,
}

export default CreatePost
```

Ce formulaire contient de quoi créer les 3 champs d'un post. La première ligne de ce composant fait appel à un hook `useState`. Ceci va permettre de gérer l'état local du composant.

2 éléments vont faire partie de cet état : `post` et `message`. `message` sert à notifier l'utilisateur en cas d'erreur ou de succès à la soumission. `post` contient les informations remplies par l'utilisateur et structurées selon l'objet `initialState`.

Lors de l'utilisation de `useState`, 2 variables sont déclarées : la variable contenant l'état local (par exemple `post`) et une fonction (`setPost` dans l'exemple), qui remplace le traditionnel `setState` d'un composant avec classe.

Pour illustrer l'avantage des hooks, voici ce qu'aurait donné le même composant s'il avait été une classe :

src/post/CreatePost.js
```js
import React from 'react'
import PropTypes from 'prop-types'

class CreatePost extends React.component => {
  constructor(props) {
    super(props)
    this.state = {
      content: '',
      tags: [],
      author: '',
      message: ''
    }

    this.handleInputChange = this.handleInputChange.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }

  const handleInputChange = event => {
    const { name, value } = event.target
    this.setState({ message: '' })
    document.getElementById('content').classList.remove('validate-fail')
    this.setState({ [name]: value })
  }

  const handleSubmit = event => {
    event.preventDefault()
    if (!this.state.content) {
      this.setState({ message: 'Missing field content.' })
      document.getElementById('message').classList.add('message-fail')
      document.getElementById('content').classList.add('validate-fail')
      return
    }

    this.setState({ tags = [this.state.tags] })
    const post = {
      content: this.state.content,
      author: this.state.author,
      tags: this.state.tags
    }
    props.addPost(post, props.setPosts)
    this.setState(initialState)
    this.setState({ message: 'Post added.' })
    document.getElementById('message').classList.add('message-success')
  }
  
  render () {
    return (
      <form onSubmit={handleSubmit} className="create-post">
        <div className="create-post__content">
          <label htmlFor="content">Content</label>
          <textarea
            name="content"
            id="content"
            value={post.content}
            onChange={handleInputChange}
          />
        </div>
        <div className="create-post__tags">
          <label htmlFor="tags">Tags</label>
          <input
            type="text"
            name="tags"
            id="tags"
            value={post.tags}
            onChange={handleInputChange}
          />
        </div>
        <div className="create-post__author">
          <label htmlFor="author">Author</label>
          <input
            type="text"
            name="author"
            id="author"
            value={post.author}
            onChange={handleInputChange}
          />
        </div>
        <br />
        <input type="submit" data-testid="submit" value="Submit" />
        <label id="message">{message}</label>
      </form>
    )
  }
}

CreatePost.propTypes = {
  content: PropTypes.string,
  tags: PropTypes.array,
  author: PropTypes.string,
}

export default CreatePost
```

Plus long à écrire pour le même résultat. Et encore, nous n'avons pas utilisé tous les éléments du cycle de vie d'un composant React ici (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`, et toutes les autres nouveautés). Avec les hooks, plus besoin de retenir cette litanie d'événements, ils sont déjà optimisés pour ces cas particuliers.

Le dernier composant à examiner est `App`, qui contient `addPost`. Pour conserver de la simplicité, je l'ai passé en propriété de `CreatePost`. Mais on aurait pu utiliser [un autre hook, `useContext`](https://reactjs.org/docs/hooks-reference.html#usecontext), s'évitant d'utiliser Redux dans une appli plus fournie et sa cohorte de wrappers à ajouter aux classes.

src/App.js
```js
import React, { useState } from 'react'
import CreatePost from './post/CreatePost'
import Posts from './post/Posts'
import postsData from './data/fixtures'

const App = () => {
  const [posts, setPosts] = useState(postsData)

  const addPost = post => {
    post.id = posts.length + 1
    setPosts([...posts, post])
  }

  const sortPosts = posts => setPosts(posts)

  return (
    <div className="container">
      <h1>Posts app</h1>
      <div className="flex-row">
        <div className="flex-large">
          <h2>Add post</h2>
          <CreatePost addPost={addPost} />
        </div>
        <div className="flex-large">
          <h2>View posts</h2>
          <Posts posts={posts} sortPosts={sortPosts} />
        </div>
      </div>
    </div>
  )
}

export default App
```

J'ai ajouté des fixtures pour avoir quelques posts à disposition et une feuille de style pour avoir ce rendu :

![](https://raw.githubusercontent.com/falkodev/react-hooks/master/picture.png)

On a ainsi une application qui aurait demandé auparavant des composants en classe, plus long à écrire et plus complexes. Avec la simplicité des composants en fonction, la lisibilité est meilleure et on peut gérer l'état de la même manière.

Cette fonctionnalité est encore en phase alpha de développement. L'équipe React recueille des avis, et va certainement modifier certains comportements dans les mois à venir. C'est pour cela qu'il n'est pas recommandé d'utiliser les hooks en production, ou même de refactoriser des classes en fonctions. C'est tout de même très prometteur pour avoir des applications encore plus simples à développer et maintenir.

Dans la deuxième partie, nous examinerons comment tester ces composants. Les sources de cette appli sont disponibles [ici](https://github.com/falkodev/react-hooks).
# Alert Libraries

There are a couple of good libraries.  I am liking [SweetAlert](https://sweetalert.js.org/) and [SweetAlert2](https://sweetalert2.github.io/) at the moment.

##  SweetAlert and SweetAlert 2

Great packages.  SweetAlert2 is a fork of SweetAlert. Both seem to be maintained, however SweetAlert2 has much more/better docs.  That is what you should use.

```
yarn add sweetalert2
```



### Examples

#### Delete Confirmation Dialog

This guy also shows an image using the "html" attribute

```javascript
import swal from 'sweetalert2';

const onDeleteShow = () => {
    swal({
      title: 'Are you sure?',
      text: "You won't be able to revert this!",
      html: `<img height="200" src=${props.showData.posterPath} />`,
      showCancelButton: true,
      confirmButtonColor: '#3085d6',
      cancelButtonColor: '#d33',
      confirmButtonText: 'Yes, delete it!'
    }).then((result) => {
      if (result.value) {
        props.startDeleteTVShow(props.showData.showId)
         .then(() => swal(
          'Deleted!',
          'Your file has been deleted.',
          'success'
        ))
      }
    });
}
```

Here is a different way to use an image:

```javascript
swal({
  title: 'Sweet!',
  text: 'Modal with a custom image.',
  imageUrl: 'https://unsplash.it/400/200',
  imageWidth: 400,
  imageHeight: 200,
  imageAlt: 'Custom image',
  animation: false
})
```



It can do the standard alerts, with actions to take when OK is selected, etc.  But it can also accept input along a change of alerts and return the results.

Look at the documentation page for "Chaining Modals"

```javascript
swal.setDefaults({
  input: 'text',
  confirmButtonText: 'Next &rarr;',
  showCancelButton: true,
  progressSteps: ['1', '2', '3']
})

var steps = [
  {
    title: 'Question 1',
    text: 'Chaining swal2 modals is easy'
  },
  'Question 2',
  'Question 3'
]

swal.queue(steps).then((result) => {
  swal.resetDefaults()

  if (result.value) {
    swal({
      title: 'All done!',
      html:
        'Your answers: <pre>' +
          JSON.stringify(result.value) +
        '</pre>',
      confirmButtonText: 'Lovely!'
    })
  }
})
```


# Alert Libraries

There are a couple of good libraries.  I am liking [Sweet Alert](https://sweetalert2.github.io/) at the moment.

## Sweet Alert 2

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


---

# required metadata
title: "Using foreach and iterators"
description: "High level guide to using foreach and iterators packages."
keywords: ""
author: "HeidiSteen"
manager: "jhubbard"
ms.date: "04/20/2016"
ms.topic: "article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: "r-server"
ms.custom: ""

---

# Parallelizing Loops

One common approach to parallelization is to see if the iterations
within a loop can be performed independently, and if so, then try to run
the iterations concurrently rather than sequentially. can help you do
this loop parallelization quickly and easily.

## Using `foreach`


The `foreach` package is a set of tools that allow you to run virtually
anything that can be expressed as a for-loop as a set of parallel tasks.
One application of this is to allow multiple simulations to run in
parallel. As a simple example, consider the case of simulating 10000
coin flips, which can be done by sampling with replacement from the
vector `c(H, T)`. To run this simulation 10 times sequentially, use
`foreach` with the `%do%` operator:

    > library(foreach)
    > foreach(i=1:10) %do% sample(c("H", "T"), 10000, replace=TRUE)

Comparing the `foreach` output with that of a similar `for` loop shows
one obvious difference: `foreach` returns a list containing the value
returned by each computation. A `for` loop, by contrast, returns only
the value of its last computation, and relies on user-defined side
effects to do its work.

We can parallelize the operation immediately by replacing `%do%` with
`%dopar%`:

    > foreach(i=1:10) %dopar% sample(c("H", "T"), 10000, replace=TRUE)

However, if we run this example, we see the following warning:

    Warning message:
    executing %dopar% sequentially: no parallel backend registered

To actually run in parallel, we need to have a “parallel backend” for
`foreach`. Parallel backends are discussed in the next section.

## Parallel Backends

In order for loops coded with `foreach` to run in parallel, you must
register a parallel backend to manage the execution of the loop. Any
type of mechanism for running code in parallel could potentially have a
parallel backend written for it. Currently, Microsoft
includes the `doParallel` backend; this uses the `parallel` package of R
2.14.0 or later to run jobs in parallel, using either of the component
parallelization methods incorporated into the parallel package:
SNOW-like functionality using socket connections or multicore-like
functionality using forking (on Linux only).

The `doParallel` package is a parallel backend for `foreach` that is
intended for parallel processing on a single computer with multiple
cores or processors.

Additional parallel backends are available from CRAN:

-   `doMPI` for use with the Rmpi package

-   `doRedis` for use with the rredis package

-   `doMC` provides access to the multicore functionality of the
    parallel package

-   `doSNOW` for use with the now superseded SNOW package.

To use a parallel backend, you must first register it. Once a parallel
backend is registered, calls to `%dopar%` run in parallel using the
mechanisms provided by the parallel backend. However, the details of
registering the parallel backends differ, so we consider them
separately.

### Using the `doParallel` parallel backend

The parallel package of R 2.14.0 and later combines elements of snow and
multicore; doParallel similarly combines elements of both doSNOW and
doMC. You can register doParallel with a cluster, as with doSNOW, or
with a number of cores, as with doMC. For example, here we create a
cluster and register it:

    > library(doParallel)
    > cl <- makeCluster(4)
    > registerDoParallel(cl)

Once you’ve registered the parallel backend, you’re ready to run
`foreach` code in parallel. For example, to see how long it takes to run
10,000 bootstrap iterations in parallel on all available cores, you can
run the following code:

    > x <- iris[which(iris[,5] != "setosa"), c(1,5)]
    > trials <- 10000
    > ptime <- system.time({
    +     r <- foreach(icount(trials), .combine = cbind) %dopar% {
    +         ind <- sample(100, 100, replace = TRUE)
    +         result1 <- glm(x[ind, 2] ~ x[ind, 1], family=binomial(logit))
    +         coefficients(result1)
    +    }
    + })[3]
    > ptime

### Getting information about the parallel backend

To find out how many workers `foreach` is going to use, you can use the
`getDoParWorkers` function:

    > getDoParWorkers()

This is a useful sanity check that you’re actually running in parallel.
If you haven’t registered a parallel backend, or if your machine only
has one core, `getDoParWorkers` will return 1. In either case, don’t
expect a speed improvement.

The `getDoParWorkers` function is also useful when you want the number
of tasks to be equal to the number of workers. You may want to pass this
value to an iterator constructor, for example.

You can also get the name and version of the currently registered
backend:

    > getDoParName()
    > getDoParVersion()

## Nesting Calls to `foreach`


An important feature of `foreach` is nesting operator `%:%`. Like the
`%do%` and `%dopar%` operators, it is a binary operator, but it operates
on two `foreach` objects. It also returns a `foreach` object, which is
essentially a special merger of its operands.

Let’s say that we want to perform a Monte Carlo simulation using a
function called `sim`. The `sim` function takes two arguments, and we
want to call it with all combinations of the values that are stored in
the vectors `avec` and `bvec`. The following doubly-nested `for` loop
does that. For testing purposes, the `sim` function is defined to return
$10 a + b$ (although an operation this trivial is not worth executing in
parallel):

    sim <- function(a, b) 10 * a + b
    avec <- 1:2
    bvec <- 1:4

    x <- matrix(0, length(avec), length(bvec))
    for (j in 1:length(bvec)) {
          for (i in 1:length(avec)) {
                  x[i,j] <- sim(avec[i], bvec[j])
                    }
    }
    x

In this case, it makes sense to store the results in a matrix, so we
create one of the proper size called `x`, and assign the return value of
`sim` to the appropriate element of `x` each time through the inner
loop.

When using `foreach`, we don’t create a matrix and assign values into
it. Instead, the inner loop returns the columns of the result matrix as
vectors, which are combined in the outer loop into a matrix. Here’s how
to do that using the `%:%` operator:

    x <-
      foreach(b=bvec, .combine='cbind') %:%
          foreach(a=avec, .combine='c') %do% {
              sim(a, b)
          }
    x

This is structured very much like the nested `for` loop. The outer
`foreach` is iterating over the values in “bvec”, passing them to the
inner `foreach`, which iterates over the values in “avec” for each value
of “bvec”. Thus, the “sim” function is called in the same way in both
cases. The code is slightly cleaner in this version, and has the
advantage of being easily parallelized.

When parallelizing nested `for` loops, there is always a question of
which loop to parallelize. The standard advice is to parallelize the
outer loop. This results in larger individual tasks, and larger tasks
can often be performed more efficiently than smaller tasks. However, if
the outer loop doesn’t have many iterations and the tasks are already
large, parallelizing the outer loop results in a small number of huge
tasks, which may not allow you to use all of your processors, and can
also result in load balancing problems. You could parallelize an inner
loop instead, but that could be inefficient because you’re repeatedly
waiting for all the results to be returned every time through the outer
loop. And if the tasks and number of iterations vary in size, then it’s
really hard to know which loop to parallelize.

But in our Monte Carlo example, all of the tasks are completely
independent of each other, and so they can all be executed in parallel.
You really want to think of the loops as specifying a single stream of
tasks. You just need to be careful to process all of the results
correctly, depending on which iteration of the inner loop they came
from.

That is exactly what the `%:%` operator does: it turns multiple
`foreach` loops into a single loop. That is why there is only one `%do%`
operator in the example above. And when we parallelize that nested
`foreach` loop by changing the `%do%` into a `%dopar%`, we are creating
a single stream of tasks that can all be executed in parallel:

    x <-
      foreach(b=bvec, .combine='cbind') %:%
          foreach(a=avec, .combine='c') %dopar% {
              sim(a, b)
          }
    x

Of course, we’ll actually only run as many tasks in parallel as we have
processors, but the parallel backend takes care of all that. The point
is that the `%:%` operator makes it easy to specify the stream of tasks
to be executed, and the `.combine` argument to `foreach` allows us to
specify how the results should be processed. The backend handles
executing the tasks in parallel.

For more on nested `foreach` calls, see the vignette “Nesting `foreach`
Loops” in the `foreach` package.

## Using Iterators

An <span>*iterator*</span> is a special type of object that generalizes
the notion of a looping variable. When passed as an argument to a
function that knows what to do with it, the iterator supplies a sequence
of values. The iterator also maintains information about its state, in
particular its current index.

includes a number of functions for creating iterators, the simplest of
which is `iter`, which takes virtually any R object and turns it into an
iterator object. The simplest function that operates on iterators is the
`nextElem` function, which when given an iterator, returns the next
value of the iterator. For example, here we create an iterator object
from the sequence 1 to 10, and then use `nextElem` to iterate through
the values:

    > i1 <- iter(1:10)
    > nextElem(i1)
    [1] 1
    > nextElem(i1)
    [2] 2

You can create iterators from matrices and data frames, using the `by`
argument to specify whether to iterate by row or column:

    > istate <- iter(state.x77, by='row')
    > nextElem(istate)
            Population Income Illiteracy Life Exp Murder HS Grad Frost  Area
    Alabama       3615   3624        2.1    69.05   15.1    41.3    20 50708
    > nextElem(istate)
           Population Income Illiteracy Life Exp Murder HS Grad Frost   Area
    Alaska        365   6315        1.5    69.31   11.3    66.7   152 566432

Iterators can also be created from functions, in which case the iterator
can be an endless source of values:

    > ifun <- iter(function() sample(0:9, 4, replace=TRUE))
    > nextElem(ifun)
    [1] 9 5 2 8
    > nextElem(ifun)
    [1] 3 4 2 2

For practical applications, iterators can be paired with `foreach` to
obtain parallel results quite easily:

    > x <- matrix(rnorm(1000000), ncol=1000)
    > itx <- iter(x, by='row')
    > foreach(i=itx, .combine=c) %dopar% mean(i)

### Some Special Iterators

The notion of an iterator is new to R, but should be familiar to users
of languages such as Python. The `iterators` package includes a number
of special functions that generate iterators for some common scenarios.
For example, the `irnorm` function creates an iterator for which each
value is drawn from a specified random normal distribution:

    > library(iterators)
    > itrn <- irnorm(1, count=10)
    > nextElem(itrn)
    [1] 0.6300053
    > nextElem(itrn)
    [1] 1.242886

Similarly, the `irunif`, `irbinom`, and `irpois` functions create
iterators which drawn their values from uniform, binomial, and Poisson
distributions, respectively. (These functions use the standard R
distribution functions to generate random numbers, and these are not
necessarily useful in a distributed or parallel environment. When using
random numbers with `foreach`, we recommend using the doRNG package to
ensure independent random number streams on each worker.)

We can then use these functions just as we used `irnorm`:

    > itru <- irunif(1, count=10)
    > nextElem(itru)
    [1] 0.4960539
    > nextElem(itru)
    [1] 0.4071111

The `icount` function returns an iterator that counts starting from one:

    > it <- icount(3)
    > nextElem(it)
    [1] 1
    > nextElem(it)
    [1] 2
    > nextElem(it)
    [1] 3

### Writing Iterators

There will be times when you need an iterator that isn’t provided by the
`iterators` package. That is when you need to write your own custom
iterator.

Basically, an iterator is an S3 object whose base class is `iter`, and
has `iter` and `nextElem` methods. The purpose of the `iter` method is
to return an iterator for the specified object. For iterators, that
usually just means returning itself, which seems odd at first. But the
`iter` method can be defined for other objects that don’t define a
`nextElem` method. We call those objects <span>*iterables*</span>,
meaning that you can iterate over them. The `iterators` package defines
`iter` methods for vectors, lists, matrices, and data frames, making
those objects iterables. By defining an `iter` method for iterators,
they can be used in the same context as an iterable, which can be
convenient. For example, the `foreach` function takes iterables as
arguments. It calls the `iter` method on those arguments in order to
create iterators for them. By defining the `iter` method for all
iterators, we can pass iterators to `foreach` that we created using any
method we choose. Thus, we can pass vectors, lists, or iterators to
`foreach`, and they are all processed by `foreach` in exactly the same
way.

The `iterators` package comes with an `iter` method defined for the
`iter` class that simply returns itself. That is usually all that is
needed for an iterator. However, if you want to create an iterator for
some existing class, you can do that by writing an `iter` method that
returns an appropriate iterator. That will allow you to pass an instance
of your class to `foreach`, which will automatically convert it into an
iterator. The alternative is to write your own function that takes
arbitrary arguments, and returns an iterator. You can choose whichever
method is most natural.

The most important method required for iterators is `nextElem`. This
simply returns the next value, or throws an error. Calling the `stop`
function with the string `StopIteration` indicates that there are no
more values available in the iterator.

In most cases, you don’t actually need to write the `iter` and
`nextElem` methods; you can inherit them. By inheriting from the class
`abstractiter`, you can use the following methods as the basis of your
own iterators:

    > iterators:::iter.iter
    function (obj, ...)
    {
            obj
    }
    <environment: namespace:iterators>
    > iterators:::nextElem.abstractiter
    function (obj, ...)
    {
            obj$nextElem()
    }
    <environment: namespace:iterators>

The following function creates a simple iterator that uses these two
methods:

    iforever <- function(x) {
        nextEl <- function() x
        obj <- list(nextElem=nextEl)
        class(obj) <- c('iforever', 'abstractiter', 'iter')
        obj
    }

Note that we called the internal function `nextEl` rather than
`nextElem` to avoid masking the standard `nextElem` generic function.
That causes problems when you want your iterator to call the `nextElem`
method of another iterator, which can be quite useful.

We create an instance of this iterator by calling the `iforever`
function, and then use it by calling the `nextElem` method on the
resulting object:

    it <- iforever(42)
    nextElem(it)
    nextElem(it)

Notice that it doesn’t make sense to implement this iterator by defining
a new `iter` method, since there is no natural iterable on which to
dispatch. The only argument that we need is the object for the iterator
to return, which can be of any type. Instead, we implement this iterator
by defining a normal function that returns the iterator.

This iterator is quite simple to implement, and possibly even useful. Be
careful, however, how you you use this iterator. If you pass it to
`foreach`, it will result in an infinite loop unless you pair it with a
finite iterator. And never pass this iterator to `as.list` without the
`n` argument.

The iterator returned by `iforever` is a list that has a single element
named `nextElem`, whose value is a function that returns the value of
`x`. Because we are subclassing `abstractiter`, we inherit a `nextElem`
method that will call this function, and because we are subclassing
`iter`, we inherit an `iter` method that will return itself.

Of course, the reason this iterator is so simple is because it doesn’t
contain any state. Most iterators need to contain some state, or it will
be difficult to make it return different values and eventually stop.
Managing the state is usually the real trick to writing iterators.

As an example of writing a stateful iterator, Let’s modify the previous
iterator to put a limit on the number of values that it returns. I’ll
call the new function `irep`, and give it another argument called
`times`:

    irep <- function(x, times) {
        nextEl <- function() {
            if (times > 0) {
                times <<- times - 1
            }
            else {
                stop('StopIteration')
            }
            x
        }
        obj <- list(nextElem=nextEl)
        class(obj) <- c('irep', 'abstractiter', 'iter')
        obj
    }

Now let’s try it out:

    it <- irep(7, 6)
    unlist(as.list(it))

The real difference between `iforever` and `irep` is in the function
that gets called by the `nextElem` method. This function not only
accesses the values of the variables `x` and `times`, but it also
modifies the value of `times`. This is accomplished by means of the
`<<-` operator, and the magic of lexical scoping. Technically, this kind
of function is called a <span>*closure*</span>, and is a somewhat
advanced feature of `R`. The important thing to remember is that
`nextEl` is able to get the value of variables that were passed as
arguments to `irep`, and it can modify those values using the `<<-`
operator. These are <span>*not*</span> global variables: they are
defined in the enclosing environment of the `nextEl` function. You can
create as many iterators as you want using the `irep` function, and they
will all work as expected without conflicts.

Note that this iterator only uses the arguments to `irep` to store its
state. If any other state variables are needed, they can be defined
anywhere inside the `irep` function.

More examples of writing iterators can be found in the vignette “Writing
Custom Iterators” in the `iterators` package.

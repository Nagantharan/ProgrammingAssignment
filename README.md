# ProgrammingAssignment2
Programming Assignment 2: Lexical Scoping

## Pair of functions that are used to create a special object that 
## stores a matrix and caches its inverse.


## This first function creates a special "matrix" object that can cache its inverse.

makeCacheMatrix <- function(x = matrix()) {

  i <- NULL
  set <- function(y) {
    
    x <<- y
    i <<- NULL
    
  }
  
  get <- function() x
  setInverse <- function(inverse) i <<- inverse
  getInverse <- function() i
  list(set = set,
       get = get,
       setInverse = setInverse,
       getInverse = getInverse)
  
}


## This second function computes the inverse of the special "matrix" created by 
## makeCacheMatrix above. If the inverse has already been calculated (and the 
## matrix has not changed), then it should retrieve the inverse from the cache.

cacheSolve <- function(x, ...) {
        ## Return a matrix that is the inverse of 'x'
  
  i <- x$getInverse()
  if (!is.null(i)) {
    message("getting cached data")
    return(i)
  }
  mat <- x$get()
  i <- solve(mat, ...)
  x$setInverse(i)
  i
}

## Caching the Inverse of a Matrix:
## Matrix inversion is usually a costly computation and there may be some 
## benefit to caching the inverse of a matrix rather than compute it repeatedly.
## Below are a pair of functions that are used to create a special object that 
## stores a matrix and caches its inverse.

## This function creates a special "matrix" object that can cache its inverse.

makeCacheMatrix <- function(x = matrix()) {
        inv <- NULL
        set <- function(y) {
                x <<- y
                inv <<- NULL
        }
        get <- function() x
        setInverse <- function(inverse) inv <<- inverse
        getInverse <- function() inv
        list(set = set,
             get = get,
             setInverse = setInverse,
             getInverse = getInverse)
}


## This function computes the inverse of the special "matrix" created by 
## makeCacheMatrix above. If the inverse has already been calculated (and the 
## matrix has not changed), then it should retrieve the inverse from the cache.

cacheSolve <- function(x, ...) {
        ## Return a matrix that is the inverse of 'x'
        inv <- x$getInverse()
        if (!is.null(inv)) {
                message("getting cached data")
                return(inv)
        }
        mat <- x$get()
        inv <- solve(mat, ...)
        x$setInverse(inv)
        inv
}

> source("ProgrammingAssignment2/cachematrix.R")
> my_matrix <- makeCacheMatrix(matrix(1:4, 2, 2))
> my_matrix$get()
     [,1] [,2]
[1,]    1    3
[2,]    2    4
> my_matrix$getInverse()
NULL
> cacheSolve(my_matrix)
     [,1] [,2]
[1,]   -2  1.5
[2,]    1 -0.5
> cacheSolve(my_matrix)
getting cached data
     [,1] [,2]
[1,]   -2  1.5
[2,]    1 -0.5
> my_matrix$getInverse()
     [,1] [,2]
[1,]   -2  1.5
[2,]    1 -0.5
> my_matrix$set(matrix(c(2, 2, 1, 4), 2, 2))
> my_matrix$get()
     [,1] [,2]
[1,]    2    1
[2,]    2    4
> my_matrix$getInverse()
NULL
> cacheSolve(my_matrix)
           [,1]       [,2]
[1,]  0.6666667 -0.1666667
[2,] -0.3333333  0.3333333
> cacheSolve(my_matrix)
getting cached data
           [,1]       [,2]
[1,]  0.6666667 -0.1666667
[2,] -0.3333333  0.3333333
> my_matrix$getInverse()
           [,1]       [,2]
[1,]  0.6666667 -0.1666667
[2,] -0.3333333  0.3333333


open.account <- function(total) {
        list(
                deposit = function(amount) {
                        if(amount <= 0)
                                stop("Deposits must be positive!\n")
                        total <<- total + amount
                        cat(amount, "deposited.  Your balance is", total, "\n\n")
                },
                withdraw = function(amount) {
                        if(amount > total)
                                stop("You don't have that much money!\n")
                        total <<- total - amount
                        cat(amount, "withdrawn.  Your balance is", total, "\n\n")
                },
                balance = function() {
                        cat("Your balance is", total, "\n\n")
                }
        )
        
        peter <- open.account(100)
> mm <- open.account(200)
> peter$withdraw(30)
30 withdrawn.  Your balance is 70 

> peter$balance()
Your balance is 70

> mm$balance()
Your balance is 200 

> peter$deposit(50)
50 deposited.  Your balance is 120 

> peter$balance()
Your balance is 120 

> peter$withdraw(500)
Error in peter$withdraw(500) : You don't have that much money!
> mm$balance()
Your balance is 200

> mm$withdraw(12)
12 withdrawn.  Your balance is 188 

> mm$balance()
Your balance is 188 

> peter$balance()
Your balance is 120

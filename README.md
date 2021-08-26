# Rcpp_tests

```
library(Rcpp)

cppFunction(
  'List test_arma_and_or_not(
  arma::uvec aa, 
  arma::uvec bb) {
  List result=Rcpp::List::create(
    Rcpp::Named("and")=aa&&bb,
    Rcpp::Named("or")=aa||bb,
    Rcpp::Named("eqv")=aa==bb,
    Rcpp::Named("not")=aa==0);
  return(result);
}',depends="RcppArmadillo")

df<-expand.grid(aa=c(TRUE,FALSE,NA),bb=c(TRUE,FALSE,NA))
cbind(df$aa,df$bb,test_arma_and_or_not(df$aa,df$bb)$and)

cppFunction(
  'List test_arma_if_logical(
  arma::uvec aa) {
  for(int ii=0;ii<aa.n_elem;ii++){
    if(aa(ii))Rcpp::Rcout<<aa(ii)<<std::endl;
  }
  List result;
  return(result);
}',depends="RcppArmadillo")
test_arma_if_logical(c(T,F,NA))

cppFunction(
  'List test_rcpp_and_or_not(
  LogicalVector aa, 
  LogicalVector bb) {
  List result=Rcpp::List::create(
    Rcpp::Named("and")=aa&bb,
    Rcpp::Named("or")=aa|bb,
    Rcpp::Named("eqv")=aa==bb,
    Rcpp::Named("not")=!aa);
  return(result);
}')

df<-expand.grid(aa=c(TRUE,FALSE,NA),bb=c(TRUE,FALSE,NA))
cbind(df$aa,df$bb,test_rcpp_and_or_not(df$aa,df$bb)$and)

cppFunction(
  'List test_rcpp_if_logical(
  LogicalVector aa) {
  for(int ii=0;ii<aa.length();ii++){
    if(aa(ii))Rcpp::Rcout<<aa(ii)<<std::endl;
  }
  List result;
  return(result);
}')
test_arma_if_logical(c(T,F,NA))

cppFunction(
  'List test_rcpp_subset(
  NumericVector aa, 
  LogicalVector bb) {
  
  List result=Rcpp::List::create(
    Rcpp::Named("sub")=aa[bb]);
  
  return(result);
}')

sourceCpp("Rcpp/test_arma_and_or.cpp")

df<-expand.grid(aa=c(TRUE,FALSE,NA),bb=c(TRUE,FALSE,NA))

cbind(df$aa,df$bb,test_arma_and_or_not(df$aa,df$bb)$and)
cbind(df$aa,df$bb,test_rcpp_and_or_not(df$aa,df$bb)$and)

cbind(a=df$aa,b=test_arma_and_or_not(df$aa,df$bb)$not)

test_rcpp_subset(1:3,c(TRUE,FALSE,NA))
```



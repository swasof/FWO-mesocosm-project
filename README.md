
# FWO-mesocosm-project
library(dplyr)
library(vegan)
setwd("E:/Users/swasof/updated version of the project 19.04.2016/FWO Mesocosm/Data/experiment measurement and analyses/2016/soil analyses/March/Nancy-nematodes data")
data.nem <- read.table("data nem 2016.txt", h = T, sep = "\t")
data.nem <- read.table("data nem 2016.txt", h = T, sep = "\t")
vec <- colnames(data.nem[,-c(1:4, 15)])
data.nem[,vec] <- lapply(data.nem[,vec], as.numeric)
#sum <- apply(data.nem[,3:12], 2, sum, na.rm = T)
fun <- function(x)coalesce(x,0)
data.nem[,vec] <- lapply(data.nem[,vec], fun)


head(data.nem)
rownames(data.nem) <- as.character(data.nem$Genus)
data <- as.data.frame(t(data.nem[,5:14]))

soilType <- rep(0, nrow(data))
data <- data.frame(soilType, data)

data$soilType[grep(".H", rownames(data)) ] =  "Eutro"
data$soilType[grep(".M", rownames(data)) ] =  "Meso"
data$soilType[grep(".L", rownames(data)) ] =  "Oligo"
data$soilType[grep("control", rownames(data)) ] =  "control"
fun <- function (x) colSums(x[[1]][,-1])
x=split(data, data$soilType)
data1 <- sapply(x, function (x) colSums(x[,-1]))
data1 <- data.frame(FT=data.nem$FT, data1)

fun <- function(x) x/colSums(x)
data1[,2:5] <- fun(data1[,2:5])

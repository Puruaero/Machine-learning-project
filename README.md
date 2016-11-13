# Stats-project
The project in Statistical Learning
library(lubridate)
library(mice)
library(MASS)
# library(randomForest)
library(plyr)
# library(class)
library(caret)

training <- read.csv("combined.csv",na.strings=c(""," ","NA"))
# info <- read.csv("avars1.csv",na.strings=c(""," ","NA"))
# training <- merge(training, info, by.x="id", by.y="id", sort=TRUE)

training$clear <- factor(training$clear)
training$clear <- ordered(training$clear, levels=c("1", "2", "3", "4", "5"))

training$difficult <- factor(training$difficult)
training$difficult <- ordered(training$difficult, levels=c("1", "2", "3", "4", "5"))

training$interesting <- factor(training$interesting)
training$interesting <- ordered(training$interesting, levels=c("1", "2", "3", "4", "5"))

training$thinking <- factor(training$thinking)
training$thinking <- ordered(training$thinking, levels=c("1", "2", "3", "4", "5"))

training$enjoy <- factor(training$enjoy)
training$enjoy <- ordered(training$enjoy, levels=c("1", "2", "3", "4", "5"))

# training$startsec<-period_to_seconds(hms(training$starttime))
# training$endsec<-period_to_seconds(hms(training$endtime))

# training$startday<-mday(dmy(training$startdate))
# training$endday<-mday(dmy(training$enddate))

# training$month<-month(dmy(training$startdate))
# training$year<-year(dmy(training$startdate))

# training$duration[is.na(training$duration)] <- round(mean(training$duration,na.rm = TRUE))
# training$enjoy[is.na(training$enjoy)] <- round(mean(training$enjoy,na.rm = TRUE))

# training$core <- factor(training$core, levels=c(levels(training$core), "leisure"))
# training$core[is.na(training$core)] = "leisure"

training$obs <- NULL
training$train <- NULL
training$startdate <- NULL 
training$enddate <- NULL
training$starttime <- NULL
training$endtime <- NULL
training$id <- NULL
training$year_month_m <- NULL
training$core <- NULL
# training$endday[is.na(training$endday)] <- training$startday[is.na(training$endday)]

# training$startsec[is.na(training$startsec)] <- round(mean(training$startsec,na.rm = TRUE))

# x=round(mean(training$startsec,na.rm = TRUE))
train <- training[complete.cases(training), ]

# trainX <- training[,names(training) != "interesting"]
# preProcValues <- preProcess(x = trainX,method = c("center", "scale"))

ctrl <- trainControl(method="repeatedcv", repeats=3) #,classProbs=TRUE,summaryFunction = twoClassSummary)
gbmFit <- train(interesting ~ ., data = train, trctrl= ctrl, method = "knn", tuneLength = 5)
print(gbmFit)
# write.csv(train, "c.csv")

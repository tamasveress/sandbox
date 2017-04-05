setwd("D:\\rdata")
.First.sys()
##########################################
## Packages ##############################
##########################################
#https://systematicinvestor.wordpress.com/systematic-investor-toolbox/
#https://systematicinvestor.wordpress.com/shiny/
#https://cssanalytics.wordpress.com/
#setInternet2(TRUE)
#con = gzcon(url('https://github.com/systematicinvestor/SIT/raw/master/sit.gz', 'rb'))
#source(con)
#close(con)
#require(TTR)
#require(TRTH) PostgreSQL
library(data.table)
library(reshape2)
library(dplyr)
library(devtools)
#library(corrplot)
library(data.table)
require(reshape2)
require(lsr)
require(quantreg)
require(foreach)
require(doParallel)
require(markdown)
require(knitr)
require(IBrokers)
require(quantmod)
require(devtools)
require(googleCharts)
require(shinyTable)
require(shiny)
require(RPostgreSQL)
require(ggplot2)
require(mailR)
require(R2HTML)
require(scales)
require(stats)
require(quantmod)
require(Quandl)
require(data.table)
require(RColorBrewer)
require(grid)
require(gridExtra)
require(reshape2)
require(timeSeries)
require(plotrix)
require(Rcmdr)
require(googleVis)
require(mosaic)
require(R2HTML)
require(RHTMLForms)
require(RCurl)
require(rCharts) #rcom and scExcel DCOM statconnDCOM rscproxy for RExcel http://rcom.univie.ac.at/download.html#RPackages
#require(quantmod) #http://www.quintuitive.com/2012/11/30/trading-with-support-vector-machines-svm/
#require(PerformanceAnalytics )
require(zoo)
#require(gcookbook)
#require(shinyapps)
#require(shiny)
#require(rvest)
#require(bmp)(png)(jpeg)GoogleVis rChart devtools ggplot2
require(TTR)
require(forecast)
require(caTools)
require(data.table)
require(plyr)
require(Quandl)
require(XLConnect)
require(xlsx)
require(xlsxjars)
require(stringr)
require(scatterplot3d)
require(graphics)
require(car)
require(rjson)
require(RCurl)
require(RJSONIO)
require(XML)
require(XML2R)
require(gmailR)
require(MASS)
#require(rNOMADS)
#require(RDCOMClient)
#SVM
require(e1071)
require(caret)
require(FinancialInstrument)
require(blotter)
require(quantstrat)
require(foreach)
require(quandClean)
require(IKTrading)
require(shinythemes)
#require(C50)
##################################################
# FUNCTIONS   ####################################
##################################################
sendgmail <- function(to, from, subject,
                      message, attachment=NULL,
                      username, password,
                      server="smtp.gmail.com:587",
                      confirmBeforeSend=FALSE){
  # http://myrcodes.blogspot.com/2013/11/gmailr.html
  # to: a list object of length 1.  Using list("Recipient" = "recip@somewhere.net") will send the message to the address but
  #     the name will appear instead of the address.
  # from: a list object of length 1.  Same behavior as 'to'
  # subject: Character(1) giving the subject line.
  # message: Character(1) giving the body of the message
  # attachment: Character(1) giving the location of the attachment
  # username: character(1) giving the username.  If missing and you are using Windows, R will prompt you for the username.
  # password: character(1) giving the password.  If missing and you are using Windows, R will prompt you for the password.
  # server: character(1) giving the smtp server.
  # confirmBeforeSend: Logical.  If True, a dialog box appears seeking confirmation before sending the e-mail.  This is to
  #                    prevent me to send multiple updates to a collaborator while I am working interactively.

  if (!is.list(to) | !is.list(from)) stop("'to' and 'from' must be lists")
  if (length(from) > 1) stop("'from' must have length 1")
  if (length(to) > 1) stop("'send.email' currently only supports one recipient e-mail address")
  if (length(attachment) > 1) stop("'send.email' can currently send only one attachment")
  if (length(message) > 1){
    stop("'message' must be of length 1")
    message <- paste(message, collapse="\\n\\n")
  }

  if (is.null(names(to))) names(to) <- to
  if (is.null(names(from))) names(from) <- from
  if (!is.null(attachment)) if (!file.exists(attachment)) stop(paste("'", attachment, "' does not exist!", sep=""))

  if (missing(username)) username <- winDialogString("Please enter your e-mail username", "")
  if (missing(password)) password <- winDialogString("Please enter your e-mail password", "")

  require(rJython)
  rJython <- rJython()

  rJython$exec("import smtplib")
  rJython$exec("import os")
  rJython$exec("from email.MIMEMultipart import MIMEMultipart")
  rJython$exec("from email.MIMEBase import MIMEBase")
  rJython$exec("from email.MIMEText import MIMEText")
  rJython$exec("from email.Utils import COMMASPACE, formatdate")
  rJython$exec("from email import Encoders")
  rJython$exec("import email.utils")

  mail<-c(
    #Email settings
    paste("fromaddr = '", from, "'", sep=""),
    paste("toaddrs  = '", to, "'", sep=""),
    "msg = MIMEMultipart()",
    paste("msg.attach(MIMEText('", message, "'))", sep=""),
    paste("msg['From'] = email.utils.formataddr(('", names(from), "', fromaddr))", sep=""),
    paste("msg['To'] = email.utils.formataddr(('", names(to), "', toaddrs))", sep=""),
    paste("msg['Subject'] = '", subject, "'", sep=""))

  if (!is.null(attachment)){
    mail <- c(mail,
              paste("f = '", attachment, "'", sep=""),
              "part=MIMEBase('application', 'octet-stream')",
              "part.set_payload(open(f, 'rb').read())",
              "Encoders.encode_base64(part)",
              "part.add_header('Content-Disposition', 'attachment; filename=\"%s\"' % os.path.basename(f))",
              "msg.attach(part)")
  }

  #SMTP server credentials
  mail <- c(mail,
            paste("username = '", username, "'", sep=""),
            paste("password = '", password, "'", sep=""),

            #Set SMTP server and send email, e.g., google mail SMTP server
            paste("server = smtplib.SMTP('", server, "')", sep=""),
            "server.ehlo()",
            "server.starttls()",
            "server.ehlo()",
            "server.login(username,password)",
            "server.sendmail(fromaddr, toaddrs, msg.as_string())",
            "server.quit()")

  message.details <-
    paste("To:               ", names(to), " (", unlist(to), ")", "\n",
          "From:             ", names(from), " (", unlist(from), ")", "\n",
          "Using server:     ", server, "\n",
          "Subject:          ", subject, "\n",
          "With Attachments: ", attachment, "\n",
          "And the message:\n", message, "\n", sep="")

  if (confirmBeforeSend)
    SEND <- winDialog("yesnocancel", paste("Are you sure you want to send this e-mail to ", unlist(to), "?", sep=""))
  else SEND <- "YES"

  if (SEND %in% "YES"){
    jython.exec(rJython,mail)
    cat(message.details)
  }
  else cat("E-mail Delivery was Canceled by the User")
}
########################
# Read Google  #########
########################
cleanGoogleTable <- function(dat, table=1, skip=0, ncols=NA, nrows=-1, header=TRUE, dropFirstCol=NA){
  if(!is.data.frame(dat)){
    dat <- dat[[table]]
  }
  if(is.na(dropFirstCol)) {
    firstCol <- na.omit(dat[[1]])
    if(all(firstCol == ".") || all(firstCol== as.character(seq_along(firstCol)))) {
      dat <- dat[, -1]
    }
  } else if(dropFirstCol) {
    dat <- dat[, -1]
  }
  if(skip > 0){
    dat <- dat[-seq_len(skip), ]
  }
  if(nrow(dat) == 1) return(dat)
  if(nrow(dat) >= 2){
    if(all(is.na(dat[2, ]))) dat <- dat[-2, ]
  }
  if(header && nrow(dat) > 1){
    header <- as.character(dat[1, ])
    names(dat) <- header
    dat <- dat[-1, ]
  }
  # Keep only desired columns
  if(!is.na(ncols)){
    ncols <- min(ncols, ncol(dat))
    dat <- dat[, seq_len(ncols)]
  }
  # Keep only desired rows
  if(nrows > 0){
    nrows <- min(nrows, nrow(dat))
    dat <- dat[seq_len(nrows), ]
  }
  # Rename rows
  rownames(dat) <- seq_len(nrow(dat))
  dat
}
##############
readGoogleSheet <- function(url, na.string="", header=TRUE){
  stopifnot(require(XML))
  # Suppress warnings because Google docs seems to have incomplete final line
  suppressWarnings({
    doc <- paste(readLines(url), collapse=" ")
  })
  if(nchar(doc) == 0) stop("No content found")
  htmlTable <- gsub("^.*?(<table.*</table).*$", "\\1>", doc)
  ret <- readHTMLTable(htmlTable, header=header, stringsAsFactors=FALSE, as.data.frame=TRUE)
  lapply(ret, function(x){ x[ x == na.string] <- NA; x})
}
#gdoc2 <- "https://docs.google.com/spreadsheets/d/1KcDq1p6p0h28tvWIC8dJ8eN8QweTJBst2Avb49fD45c/pubhtml"
#elem <- readGoogleSheet(gdoc)
#m <- cleanGoogleTable(elem, table=1)
getData<-function(tickers,datasrc){
  for (i in 1:length(tickers)){
    cat(tickers[i],i,"\n")
    getSymbols(tickers[i],src=datasrc,
               auto.assign=getOption("getSymbols.auto.assign",TRUE),
               env=parent.frame())
  }
}
# Load FX from ducascopy
###################################
loadFXData <- function(file) {
  data <- read.csv(file, sep = ",")
  data$Time <- as.POSIXct(strptime(data$Time, format="%d.%m.%Y %H:%M:%OS"))
  data <- as.xts(data[,2:6], order.by=data$Time)
  data[data$Volume != 0,]
}
# load Google fin stock
####################################
f.get.google.intraday <- function(symbol, freq, period) {
  base.url <- "http://www.google.com/finance/getprices?"
  options.url <- paste("i=",freq,"&p=",period,"&f=d,o,h,l,c,v&df=cpct&q=", symbol, sep="")
  full.url <- paste(base.url, options.url, sep="")

  data <- read.csv(full.url, skip=7, header=F, stringsAsFactors=F)

  starting.times.idx <- which(substring(data$V1, 1, 1)=="a")
  ending.seconds.idx <- c(starting.times.idx[-1]-1, nrow(data))
  r.str.idx.use <- paste(starting.times.idx, ":", ending.seconds.idx, sep="")

  starting.times <- as.numeric(substring(data[starting.times.idx,1],2))

  data[starting.times.idx, 1] <- 0
  clean.idx <- do.call(c, lapply(seq(1, length(r.str.idx.use)), function(i) starting.times[i] + freq*as.numeric(data[eval(parse(text=r.str.idx.use[i])),1])))
  data.xts <- xts(data[,-1], as.POSIXct(clean.idx, origin="1970-01-01", tz="GMT"))

  indexTZ(data.xts) <- "America/New_York"
  colnames(data.xts) <- c("Open", "High", "Low", "Close", "Volume")

  data.xts
}
############################
# km cluster predict
############################
closest.cluster <- function(x) {
  cluster.dist <- apply(km$centers, 1, function(y) sqrt(sum((x-y)^2)))
  return(which.min(cluster.dist)[1])
}
#clusters2 <- apply(df2, 1, closest.cluster)
########################
# Dropbox
# http://lcolladotor.github.io/2014/02/05/DropboxAndGoogleDocsFromR/#.VJFrPCuUfD8
########################
########################
# Source Codes #########
########################
#source('C:/Users/tamas.veress/rdata/getweatherD.R')
#source('C:/Users/tamas.veress/rdata/getweather.R')
#source('C:/Users/tamas.veress/rdata/getDem.R')
#source('C:/Users/tamas.veress/rdata/SimMaxDem.R')
########################
#  Codes ###############
########################
########################
#  GOOGLE Intraday ###############
########################
#intraday (15 mins delay)
f.get.google.intraday <- function(symbol, freq, period) {
  base.url <- 'http://www.google.com/finance/getprices?'
  options.url <- paste('i=', freq, '&p=', period, '&f=d,o,h,l,c,v&df=cpct&q=', symbol, sep = '')
  full.url <- paste(base.url, options.url, sep = '')
  
  data <- read.csv(full.url, skip = 7, header = FALSE, stringsAsFactors = FALSE)
  
  starting.times.idx <- which(substring(data$V1, 1, 1) == 'a')
  ending.seconds.idx <- c(starting.times.idx[-1] - 1, nrow(data))
  r.str.idx.use <- paste(starting.times.idx, ':', ending.seconds.idx, sep = '')
  
  starting.times <- as.numeric(substring(data[starting.times.idx, 1], 2))
  
  data[starting.times.idx, 1] <- 0
  clean.idx <- do.call(c, lapply(seq(1, length(r.str.idx.use)),
                                 function(i) {
                                   starting.times[i] + freq * as.numeric(data[eval(parse(text = r.str.idx.use[i])), 1])
                                 })
  )
  data.xts <- xts(data[,-1], as.POSIXct(clean.idx, origin = '1970-01-01', tz = 'GMT'))
  
  indexTZ(data.xts) <- 'America/New_York'
  colnames(data.xts) <- c('Open', 'High', 'Low', 'Close', 'Volume')
  
  data.xts
}
  
  #################################################
  # send mail
  
  #################################################

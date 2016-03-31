## [10 Things You Should Know About AWS](/blog/2013/11/5/10-things-you-should-know-about-aws.html)

    

    

        

_    Authored by [Chris Fregly](http://www.linkedin.com/in/cfregly)        :  Former Netflix Streaming Platform Engineer, AWS Certified Solution Architect and Purveyor of fluxcapacitor.com.    _

    Ahead of the upcoming 2nd annual re:Invent conference, inspired by Simone Brunozzi’s recent     [    presentation    ](http://www.slideshare.net/simone.brunozzi/5-thingsyoudontknowaboutaws)     at an AWS Meetup in San Francisco, and collected from a few of my recent     [    Fluxcapacitor.com    ](http://fluxcapacitor.com)     consulting engagements, I’ve compiled a list of 10 useful time and clock-tick saving tips about AWS.    

    **1) Query AWS resource metadata**    

         ![](https://lh6.googleusercontent.com/vUvviBGJIIczKhpwlka52VeSmMzxUFLWEl9Uc5bPVjIR3bSXDLiHGwNwzlqs_SWIM84mP3I7MDSV4T6cx8jFaXPYJym3heEKQI9W7E1_4BjL4tpnPjVocjuspw)

    Can’t remember the EBS-Optimized IO throughput of your c1.xlarge cluster?  How about the size limit of an S3 object on a single PUT?      [    awsnow.info    ](http://www.awsnow.info/%20)     is the answer to all of your AWS-resource metadata questions.  Interested in integrating awsnow.info with your application?  You’re in luck.  There’s now a     [    REST API    ](http://api.awsnow.info/)    , as well!    

    Note:  These are default soft limits and will vary by account.    

    **2) Tame your S3 buckets**    

         ![](https://lh5.googleusercontent.com/TuAaOoAovyIgme7Y3kEVIjKkr1TRjyn_I2yFr18fdjiH1RVAV3vkok5utFg1ED4KbuGKeG3Zv94xsrmn7iwIAuGAKRtFky9o3vrb8m33fTdp__HX0kif9LO_GA)![](https://lh6.googleusercontent.com/_0mFHKWNp6wT_FeW1hr-Sql_7zE0SV6zp31b0fIayYa6xNMPZ8BpaRP5detEfGrVBozA37mjcLIo19MCFGMQDdjIkLikInbi36tbA40YWWZa-L3u4A92DtSCGQ)

    Delete an entire S3 bucket with a single CLI command:      

_    aws s3 rb s3://<bucket-name> --forc        e    _

    Recursively copy a local directory to S3:    

    _aws s3 cp <local-dir-name> s3://<bucket-name> --region <region-name> --recursive_    

    **3) Understand AWS cross-region dependencies**    

[Click to read more ...](/blog/2013/11/5/10-things-you-should-know-about-aws.html)

    
from ubuntu:14.04
RUN apt-get 		update
RUN apt-get install -y socat
RUN echo "This is legacy application 2" > op.txt
CMD socat -d -d TCP-LISTEN:9001,fork SYSTEM:'cat op.txt'

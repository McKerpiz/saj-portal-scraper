FROM python:3.12-slim
# <-- set unicode codepage
ENV LANG=C.UTF-8    
# <-- update packages
RUN apt-get update  
# <-- install automation browser
RUN pip install --no-cache-dir --root-user-action ignore "playwright>=1.56.0" && \
    playwright install --with-deps 
# <-- install mqtt client    
RUN pip install --no-cache-dir --root-user-action ignore "paho-mqtt>=1.6,<2.0"
# <-- temporär # install editor for devs
RUN apt-get install nano 
# <-- copy all files in addon
COPY . /         
# <-- set bash executable
RUN chmod a+x /run.sh   
# <-- create folder
RUN mkdir addon_configs 
# <-- start script
CMD [ "/run.sh" ]      

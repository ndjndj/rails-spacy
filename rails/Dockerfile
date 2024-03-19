FROM ruby:3.3.0
RUN apt-get update -qq && apt-get install -y vim 

RUN apt-get install -y python3=3.11 python3-pip
ENV PYTHONIOENCODING "utf-8"
WORKDIR /usr/local/bin/
RUN ln -s `which python3` python
RUN ln -s `which pip3` pip
RUN pip install spacy ginza 
RUN python -m spacy download ja_core_news_lg

RUN mkdir /usr/src/app 
WORKDIR /usr/src/app
COPY Gemfile /usr/src/app/Gemfile 
COPY Gemfile.lock /usr/src/app/Gemfile.lock 

RUN gem update --system 
RUN bundle update --bundler 

RUN bundle install 
COPY . /usr/src/app 

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh 
ENTRYPOINT ["entrypoint.sh"]
FROM ruby:3.3.0
RUN apt-get update -qq && apt-get install -y vim 

RUN apt-get install -y build-essential
RUN apt-get install -y curl wget zip git

RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv
RUN [ \
    "/bin/bash", "-c", \
    " \
        source ~/.bashrc \
        && echo 'export PYENV_ROOT=\"$HOME/.pyenv\"' >> ~/.bashrc \
        && echo 'command -v pyenv >/dev/null || export PATH=\"$PYENV_ROOT/bin:$PATH\"' >> ~/.bashrc \
        && echo 'eval \"$(pyenv init -)\"' >> ~/.bashrc \
        && source ~/.bashrc \
        && export CONFIGURE_OPTS=\"--enable-shared\" \
        && pyenv install 3.10.6 \
        && pyenv global 3.10.6 \
        && pip install spacy==3.6.0 \
        && pip install ginza==5.1.3 \
        && python -m spacy download ja_core_news_sm \
    "] 

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

#error: externally-managed-environment
#× This environment is externally managed
#╰─> To install Python packages system-wide, try apt install python3-xyz, where xyz is the package you are trying to install. If you wish to install a non-Debian-packaged Python #package, create a virtual environment using python3 -m venv path/to/venv. Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make sure you have python3-full installed. If #you wish to install a non-Debian packaged Python application, it may be easiest to use pipx install xyz, which will manage a virtual environment for you. Make sure you have pipx #installed. See /usr/share/doc/python3.11/README.venv for more information.
#note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python #installation or OS, by passing --break-system-packages.
#0.872 hint: See PEP 668 for the detailed specification.
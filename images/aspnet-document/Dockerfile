FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

# Install image libs
RUN apt-get update && apt-get install -y libc6-dev libgdiplus

#
# Install texlive
#
RUN apt-get update &&\
# prevent doc and man pages from being installed
# the idea is based on https://askubuntu.com/questions/129566
# remove doc files and man pages already installed
    printf 'path-exclude /usr/share/doc/*\npath-include /usr/share/doc/*/copyright\npath-exclude /usr/share/man/*\npath-exclude /usr/share/groff/*\npath-exclude /usr/share/info/*\npath-exclude /usr/share/lintian/*\npath-exclude /usr/share/linda/*\npath-exclude=/usr/share/locale/*' > /etc/dpkg/dpkg.cfg.d/01_nodoc &&\
    rm -rf /usr/share/groff/* /usr/share/info/* &&\
    rm -rf /usr/share/lintian/* /usr/share/linda/* /var/cache/man/* &&\
    rm -rf /usr/share/man &&\
    mkdir -p /usr/share/man &&\
    find /usr/share/doc -depth -type f ! -name copyright -delete &&\
    find /usr/share/doc -type f -name "*.pdf" -delete &&\
    find /usr/share/doc -type f -name "*.gz" -delete &&\
    find /usr/share/doc -type f -name "*.tex" -delete &&\
    find /usr/share/doc -type d -empty -delete || true &&\
    mkdir -p /usr/share/doc &&\
    mkdir -p /usr/share/info &&\

# install utilities
    apt-get install -f -y --no-install-recommends \
          curl \
          apt-utils \
          fonts-dejavu \
          fonts-dejavu-core \
          fonts-open-sans \
          fonts-dejavu-extra \
          fontconfig \
          xz-utils || exit 1  &&\

# install TeX Live and ghostscript as well as other tools
    apt-get install -f -y --no-install-recommends \
          cm-super \
          dvipng \
          ghostscript \
          make \
          latexmk \
          lmodern \
          poppler-utils \
          psutils \
          t1utils \
          tex-gyre \
          texlive-base \
          texlive-binaries \
          texlive-font-utils \
          texlive-fonts-recommended \
          texlive-latex-base \
          texlive-latex-recommended \
          texlive-luatex \
          texlive-pstricks \
          texlive-xetex || exit 1 &&\
# Install other necessary latex packages without
# user tree and then and clean up.
    tlmgr init-usertree || exit 1 &&\
    echo "tlmgr init-usertree done successfully" &&\
    updmap-sys || exit 1 &&\
    echo "initial updmap-sys done successfully" &&\
# tlmgr will call updmap-sys unsuccessfully for
# reasons I do not understand, so while it seemingly
# succeeds, it will still fail
    tlmgr install awesomebox \
                  babel-german \
                  background \
                  filehook \
                  fontawesome5 \
                  footmisc \
                  footnotebackref \
                  fvextra \
                  ly1 \
                  mweights \
                  needspace \
                  pagecolor \
                  siunitx \
                  sourcecodepro \
                  sourcesanspro \
                  ucharcat \
                  unicode-math \
                  xecjk || true &&\
    echo "tlmgr install completed" &&\
    cat /root/texmf/web2c/updmap.cfg >> /usr/share/texmf/web2c/updmap.cfg &&\
    rm -rf /root/texmf/tlpkg || true &&\
    rm -rf /root/texmf/web2c || true &&\
    cp -n -r /root/texmf/* /usr/share/texlive/texmf-dist/ || exit 1 &&\
    rm -rf /root/texmf || true &&\
    rm -rf /root/texmf-var || true &&\
    echo "installed packages merged and /root/texmf deleted" &&\
# update latex and font system files
    fc-cache -fv || exit 1 &&\
    echo "fc-cache succeeded" &&\
    texhash --verbose || exit 1 &&\
    echo "texhash completed successfully" &&\
    updmap-sys || exit 1 &&\
    echo "updmap-sys completed successfully" &&\
    mktexlsr --verbose || exit 1 &&\
    echo "mktexlsr succeeded" &&\
    fmtutil-sys --quiet --missing &&\
    echo "fmtutil-sys --missing completed successfully" &&\
    fmtutil-sys --quiet --all > /dev/null &&\
    echo "fmtutil-sys --all completed successfully" &&\
# delete texlive sources and other potentially useless stuff
    rm -rf /usr/share/texmf/source || true &&\
    rm -rf /usr/share/texlive/texmf-dist/source || true &&\
    rm -rf /usr/share/texlive/texmf-dist/doc/ || true &&\
    find /usr/share/texlive -type f -name "readme*.*" -delete &&\
    find /usr/share/texlive -type f -name "README*.*" -delete &&\
    rm -rf /usr/share/texlive/release-texlive.txt || true &&\
    rm -rf /usr/share/texlive/doc.html || true &&\
    rm -rf /usr/share/texlive/index.html || true &&\
    rm -rf /usr/share/texlive/texmf-dist/fonts/source &&\
    rm -rf /usr/share/texlive/texmf-dist/tex/latex/pst-poker &&\
# clean up all temporary files
    apt-get clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /etc/ssh/ssh_host_* &&\
# delete man pages and documentation
    rm -rf /usr/share/man &&\
    mkdir -p /usr/share/man &&\
    find /usr/share/doc -depth -type f ! -name copyright -delete &&\
    find /usr/share/doc -type f -name "*.pdf" -delete &&\
    find /usr/share/doc -type f -name "*.gz" -delete &&\
    find /usr/share/doc -type f -name "*.tex" -delete &&\
    find /usr/share/doc -type d -empty -delete || true &&\
    mkdir -p /usr/share/doc &&\
    rm -rf /var/cache/apt/archives &&\
    mkdir -p /var/cache/apt/archives &&\
    rm -rf /tmp/* /var/tmp/* &&\
    find /usr/share/ -type f -empty -delete || true &&\
    find /usr/share/ -type d -empty -delete || true &&\
    mkdir -p /usr/share/texmf/source &&\
    mkdir -p /usr/share/texlive/texmf-dist/source
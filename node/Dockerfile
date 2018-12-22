FROM centos:6
RUN yum -y update
RUN yum install -y gcc gcc-c++ make which xz
RUN yum install -y cairo cairo-devel cairomm-devel libjpeg-turbo-devel pango pango-devel pangomm pangomm-devel giflib-devel
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
RUN source ~/.nvm/nvm.sh && nvm install v8.11.3 && npm i -g pm2 
RUN mkdir -p /release/card/build/bak_tar /release/card/build/bak_node /release/card/deploy

RUN mkdir -p /usr/local/bin/ffmpeg
COPY ["ffmpeg-release-64bit-static.tar.xz","/usr/local/bin/ffmpeg"]
WORKDIR /usr/local/bin/ffmpeg
RUN xz -d ffmpeg-release-64bit-static.tar.xz && tar -xf ffmpeg-release-64bit-static.tar
RUN cd ffmpeg-4.0.1-64bit-static
RUN ln -s /usr/local/bin/ffmpeg/ffmpeg-4.0.1-64bit-static/ffmpeg /usr/bin/ffmpeg
RUN ln -s /usr/local/bin/ffmpeg/ffmpeg-4.0.1-64bit-static/ffprobe /usr/bin/ffprobe

RUN yum install -y libtool-ltdl.x86_64 libtool-ltdl-devel.x86_64
COPY ["ImageMagick.tar.gz","/usr/local/bin/"]
WORKDIR /usr/local/bin
RUN tar -xzf ImageMagick.tar.gz
WORKDIR /usr/local/bin/ImageMagick-7.0.8-8
RUN export CPPFLAGS=-I/usr/local/include && export export LDFLAGS=-L/usr/local/lib && export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
RUN ./configure --prefix=/usr/local --disable-static --with-modules --without-perl --without-magick-plus-plus --with-quantum-depth=8 --disable-openmp && make && make install

COPY ["jpegsrc.v9c.tar.gz","/usr/local/bin/"]
WORKDIR /usr/local/bin
RUN tar -xzf jpegsrc.v9c.tar.gz
WORKDIR /usr/local/bin/jpeg-9c
RUN ./configure --enable-shared --prefix=$CONFIGURE_PREFIX && make && make install

COPY ["libpng-1.6.34.tar.gz","/usr/local/bin/"]
WORKDIR /usr/local/bin
RUN tar -xzf libpng-1.6.34.tar.gz
WORKDIR /usr/local/bin/libpng-1.6.34
RUN ./configure --enable-shared --prefix=$CONFIGURE_PREFIX && make && make install

#update glibc from 2.12 to 2.17,for canvas
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-utils-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-static-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-common-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-devel-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/glibc-headers-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
ADD ["http://copr-be.cloud.fedoraproject.org/results/mosquito/myrepo-el6/epel-6-x86_64/glibc-2.17-55.fc20/nscd-2.17-55.el6.x86_64.rpm","/usr/local/bin/update_glibc/"]
WORKDIR /usr/local/bin/update_glibc
RUN rpm -Uvh *-2.17-55.el6.x86_64.rpm --force --nodeps

#delete installed files
WORKDIR /usr/local/bin
RUN rm -rf ImageMagick.tar.gz ImageMagick-7.0.8-8/ jpeg-9c/ jpegsrc.v9c.tar.gz libpng-1.6.34.tar.gz libpng-1.6.34/ ffmpeg/ffmpeg-release-64bit-static.tar update_glibc/*-2.17-55.el6.x86_64.rpm

COPY ["start.sh","/release/card/deploy/start.sh"]
RUN chmod +x /release/card/deploy/start.sh

EXPOSE 22 5757
WORKDIR /release/card/
ENTRYPOINT ["/release/card/deploy/start.sh"]
CMD ["dev"]
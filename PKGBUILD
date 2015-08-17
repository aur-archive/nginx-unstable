pkgname=nginx-unstable
_pkgname="nginx"
_doc_root="/srv/http/${_pkgname}"
_sysconf_path="etc"
_conf_path="${_sysconf_path}/${_pkgname}"
_tmp_path="/var/run/${_pkgname}"
_log_path="/var/log/${_pkgname}"
_user="http"
_group="http"
pkgver=1.3.13
pkgrel=1
pkgdesc="lightweight HTTP server and IMAP/POP3 proxy server"
arch=('i686' 'x86_64')
depends=('pcre' 'zlib' 'openssl')
url="http://nginx.net/"
license=('custom')
conflicts=('nginx')
provides=('nginx')
backup=("${_conf_path#/}/nginx.conf"
	"${_conf_path#/}/koi-win"
	"${_conf_path#/}/koi-utf"
	"${_conf_path#/}/win-utf"
	"${_conf_path#/}/mime.types"
	"${_conf_path#/}/fastcgi_params"
	"etc/logrotate.d/nginx")
source=("http://nginx.org/download/nginx-${pkgver}.tar.gz"
	"nginx.sh"
	"nginx.conf"
	"nginx.logrotate"
    "nginx.service")
md5sums=('cf5acd823deac63cddba3fe377a0de27'
         '2eedf9f56a1c5596efac0b191ffb2a89'
         '323a91c795f509d50cff07e526cc127f'
         'e01e6f679c5c626e909f01687a2d37d4'
         '62d494d23aef31d0b867161f9fffa6eb')

build() {
	local _src_dir="${srcdir}/${_pkgname}-${pkgver}"
	local _build_dir="${_src_dir}/objs"

	cd "${_src_dir}"
	./configure \
		--prefix="/${_conf_path}" \
		--conf-path="/${_conf_path}/${_pkgname}.conf" \
		--sbin-path="/usr/sbin/${_pkgname}" \
		--pid-path="${_tmp_path}/${_pkgname}.pid" \
		--lock-path="/var/lock/${_pkgname}.lock" \
		--http-client-body-temp-path="${_tmp_path}/client_body_temp" \
		--http-proxy-temp-path="${_tmp_path}/proxy_temp" \
		--http-fastcgi-temp-path="${_tmp_path}/fastcgi_temp" \
		--http-log-path="${_log_path}/access.log" \
		--error-log-path="${_log_path}/error.log" \
		--user="${_user}" --group="${_group}" \
		--with-mail --with-mail_ssl_module \
		--with-http_ssl_module \
		--with-http_stub_status_module \
		--with-ipv6 \
		--with-file-aio \
		--with-debug
		#--with-http_realip_module \
		#--with-http_addition_module \
		#--with-http_xslt_module \
		#--with-http_image_filter_module \
		#--with-http_geoip_module \
		#--with-http_sub_module \
		#--with-http_dav_module \
		#--with-http_flv_module \
		#--with-http_gzip_static_module \
		#--with-http_random_index_module \
		#--with-http_secure_link_module \
		#--with-http_degradation_module \
		#--with-http_perl_module \
		#--add-module=path/to/passenger/source/passenger-3.0.7/ext/nginx \

	make
	make DESTDIR="${pkgdir}" install

	sed -i'' -e "s#root\s\+\w\+;#root ${_doc_root};#g" "${pkgdir}/etc/${_pkgname}/${_pkgname}.conf"

	install -d "${pkgdir}/${_tmp_path}"

	# move default document root outside server root
	install -d "${pkgdir}/${_doc_root}"
	mv "${pkgdir}/${_conf_path}/html/"* "${pkgdir}/${_doc_root}"
	rm -rf "${pkgdir}/${_conf_path}/html"

	install -D -m555 "${srcdir}/nginx.sh" "${pkgdir}/etc/rc.d/${_pkgname}"
	install -D -m644 "${srcdir}/nginx.logrotate" "${pkgdir}/etc/logrotate.d/${_pkgname}"
	install -D -m644 "${srcdir}/nginx.conf" "${pkgdir}/etc/conf.d/${_pkgname}"
	install -D -m644 "LICENSE" "${pkgdir}/usr/share/licenses/${_pkgname}/LICENSE"
	install -D -m644 "man/nginx.8" "${pkgdir}/usr/share/man/man8/nginx.8"
	install -D -m644 "$srcdir/nginx.service" "$pkgdir/usr/lib/systemd/system/nginx.service"

}

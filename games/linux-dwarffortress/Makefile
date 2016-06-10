# Created by: Michael Williams <ports@mgwsoftware.com>
# $FreeBSD$

PORTNAME=	dwarffortress
DISTVERSION=	0.42.03
CATEGORIES=	games linux
MASTER_SITES=	http://www.bay12games.com/dwarves/
PKGNAMEPREFIX=	linux-
DISTNAME=	df_42_03_linux

MAINTAINER=	ports@mgwsoftware.com
COMMENT=	Dwarven Fortress building game with ascii graphics (Linux Version)

LICENSE=	BAY12
LICENSE_NAME=	Bay 12 Games Dwarf Fortress Licence
LICENSE_TEXT=	Copyright (c) 2002-2015.  All rights are retained by Tarn Adams, save the following:  you may redistribute the unmodified binary and accompanying files, provided you do so free of charge.  You may distribute modified text files from the data and raw folders (see the readme.txt in those folders for more information).  We'd appreciate it if you credit yourself or an alias somewhere for any modifications to prevent confusion with vanilla DF (it helps with bug reports).  If you'd like to distribute a modified version of the game or portion of the archive and are worried about copyright infringement, please contact Tarn Adams at toadyone@bay12games.com.
LICENSE_PERMS=	dist-mirror pkg-mirror auto-accept

NO_BUILD=	yes
WRKSRC=		${WRKDIR}/df_linux

USES=		tar:bzip2
USE_LINUX=	yes
USE_LINUX_APPS=	xorglibs gtk2 openal-soft libsndfile sdlimage sdlttf

ONLY_FOR_ARCHS=	i386 amd64

PORTDOCS=	README.linux readme.txt file\ changes.txt release\ notes.txt command\ line.txt
REINPLACE_ARGS=

OPTIONS_DEFINE=	NVIDIA DOCS
NVIDIA_DESC=	support for nvidia

.include <bsd.port.options.mk>
.if ${PORT_OPTIONS:MNVIDIA}
RUN_DEPENDS+=	${LINUXBASE}/usr/lib/libGL.so.1:x11/nvidia-driver
USE_LINUX_APPS+=libglu
.else
USE_LINUX_APPS+=dri
.endif

do-build:
	${BRANDELF} -t Linux ${WRKSRC}/libs/Dwarf_Fortress

do-install:
	${MKDIR} ${STAGEDIR}${PREFIX}/bin
	${REINPLACE_CMD} -e "s|PREFIX|${PREFIX}|" ${SCRIPTDIR}/dwarffortress \
		> ${STAGEDIR}${PREFIX}/bin/dwarffortress
	${MKDIR} ${STAGEDIR}${PREFIX}/libexec/dwarffortress
	${INSTALL_PROGRAM} ${WRKSRC}/libs/Dwarf_Fortress ${STAGEDIR}${PREFIX}/libexec/dwarffortress
.for FILE in libgcc_s.so.1  libgraphics.so libstdc++.so.6
	${INSTALL_LIB} ${WRKSRC}/libs/${FILE} ${STAGEDIR}${PREFIX}/libexec/dwarffortress
.endfor
	${MKDIR} ${STAGEDIR}${DATADIR}
	(cd ${WRKSRC} && ${COPYTREE_SHARE} ./raw ${STAGEDIR}${DATADIR})
	${MKDIR} ${STAGEDIR}${DATADIR}/data
.for DATA in announcement art dipscript help index init initial_movies movies shader.fs shader.vs sound speech
	(cd ${WRKSRC}/data && ${COPYTREE_SHARE} ./${DATA} ${STAGEDIR}${DATADIR}/data)
.endfor
.if ${PORT_OPTIONS:MDOCS}
	${MKDIR} ${STAGEDIR}${DOCSDIR}
.for FILE in ${PORTDOCS}
	${INSTALL_DATA} ${WRKSRC}/${FILE} ${STAGEDIR}${DOCSDIR}
.endfor
.endif

.include <bsd.port.mk>

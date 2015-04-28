#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=perl-xml-parser
PKG_VERSION:=2.44
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://www.cpan.org/authors/id/T/TO/TODDR/
PKG_SOURCE:=XML-Parser-$(PKG_VERSION).tar.gz
PKG_MD5SUM:=af4813fe3952362451201ced6fbce379

PKG_LICENSE:=GPL-1.0+ Artistic-1.0-Perl
PKG_MAINTAINER:=Marcel Denia <naoir@gmx.net>

PKG_BUILD_DIR:=$(BUILD_DIR)/perl/XML-Parser-$(PKG_VERSION)

EXPAT_LIBPATH:=$(STAGING_DIR)/usr/lib
EXPAT_INCPATH=$(STAGING_DIR)/usr/include

include $(INCLUDE_DIR)/package.mk
include ../perl/perlmod.mk

define Package/perl-xml-parser
  SUBMENU:=Perl
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Perl XML Parser
  URL:=http://search.cpan.org/dist/XML-Parser/
  DEPENDS:=perl +libexpat
endef

define Build/Configure
	$(call perlmod/Configure,EXPATLIBPATH=$(EXPAT_LIBPATH) EXPATINCPATH=$(EXPAT_INCPATH),,)
	
	# Manually reconfigure XML::Parser::Expat, as some overrides we provide will not have any effect otherwise
	# Force usage of OpenWRT's expat
	sed \
		-e 's!%%EXPAT_LIBPATH%%!$(EXPAT_LIBPATH)!g' \
		-e 's!%%EXPAT_INCPATH%%!$(EXPAT_LIBPATH)!g' \
		-i $(PKG_BUILD_DIR)/Expat/Makefile.PL
	# Do it
	$(call perlmod/Configure,,,$(PKG_BUILD_DIR)/Expat)
endef

define Build/Compile
	$(call perlmod/Compile,,)
	$(call perlmod/Compile,,,$(PKG_BUILD_DIR)/Expat)
endef

define Package/perl-xml-parser/install
	$(call perlmod/Install,$(1),auto/XML XML)
endef


$(eval $(call BuildPackage,perl-xml-parser))

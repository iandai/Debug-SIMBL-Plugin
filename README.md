Debug Simbl plugin
=========

It's hard to debug simbl plugin. Without debug tool, it is difficult to build applications. 
[This article](http://d.hatena.ne.jp/griffin-stewie/20090802/p1) introduced a way to debug simbl plugin.
Because it's written five years ago, Xcode related contents have been outdated. 
Updated with Xcode 5 version.


Steps
-------
1. Change Run executable
From menu, select "Project" > "Scheme" > "Edit Scheme" and click.
From popout window, select "Run" section, change executable path to your debug application.


2. Add Run Script Build Phase
Select your project in Xcode 5, then select "Build Phases" tab.
From menu, select "Editor" > "Add Build Phase" > "Add Run Script Build Phase".
Add following script.

```
# clean up any previous products/symbolic links in the SIMBL Plugins folder
if [ -a "${USER_LIBRARY_DIR}/Application Support/SIMBL/Plugins/${FULL_PRODUCT_NAME}" ]; then
rm -Rf "${USER_LIBRARY_DIR}/Application Support/SIMBL/Plugins/${FULL_PRODUCT_NAME}"
fi

# Depending on the build configuration, either copy or link to the most recent product
if [ "${CONFIGURATION}" == "Debug" ]; then
# if we're debugging, add a symbolic link to the plug-in
ln -sf "${TARGET_BUILD_DIR}/${FULL_PRODUCT_NAME}" \
"${USER_LIBRARY_DIR}/Application Support/SIMBL/Plugins/${FULL_PRODUCT_NAME}"
elif [ "${CONFIGURATION}" == "Release" ]; then
# if we're compiling for release, just copy the plugin to the SIMBL Plugins folder
cp -Rfv "${TARGET_BUILD_DIR}/${FULL_PRODUCT_NAME}" \
"${USER_LIBRARY_DIR}/Application Support/SIMBL/Plugins/${FULL_PRODUCT_NAME}"
fi
```

3. Run and Enjoy debugging. 

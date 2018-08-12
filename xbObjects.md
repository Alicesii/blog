---
title: 4-基于库的继承
comments: true
date: 2018-07-06 16:01:30
categories: 工具
tags: 插件

---
```
/*
 * $Id: xbObjects.js,v 1.8 2003/09/14 21:22:26 bc Exp $
 *
 */

/* ***** BEGIN LICENSE BLOCK *****
 * The contents of this file are subject to the Mozilla Public License Version
 * 1.1 (the "License"); you may not use this file except in compliance with
 * the License. You may obtain a copy of the License at
 * http://www.mozilla.org/MPL/
 *
 * Software distributed under the License is distributed on an "AS IS" basis,
 * WITHOUT WARRANTY OF ANY KIND, either express or implied. See the License
 * for the specific language governing rights and limitations under the
 * License.
 *
 * Software distributed under the License is distributed on an "AS IS" basis,
 * WITHOUT WARRANTY OF ANY KIND, either express or implied. See the License
 * for the specific language governing rights and limitations under the
 * License.
 *
 * The Original Code is Bob Clary code.
 *
 * The Initial Developer of the Original Code is
 * Bob Clary.
 * Portions created by the Initial Developer are Copyright (C) 2000
 * the Initial Developer. All Rights Reserved.
 *
 * Contributor(s): Bob Clary <http://bclary.com/>
 *
 * Alternatively, the contents of this file may be used under the terms of
 * either the GNU General Public License Version 2 or later (the "GPL"), or
 * the GNU Lesser General Public License Version 2.1 or later (the "LGPL"),
 * in which case the provisions of the GPL or the LGPL are applicable instead
 * of those above. If you wish to allow use of your version of this file only
 * under the terms of either the GPL or the LGPL, and not to allow others to
 * use your version of this file under the terms of the MPL, indicate your
 * decision by deleting the provisions above and replace them with the notice
 * and other provisions required by the GPL or the LGPL. If you do not delete
 * the provisions above, a recipient may use your version of this file under
 * the terms of any one of the MPL, the GPL or the LGPL.
 *
 ***** END LICENSE BLOCK ***** */

function _Classes()
{
  if (typeof(_classes) != 'undefined')
    throw('Only one instance of _Classes() can be created');
  function registerClass(className, parentClassName)
  {
    if (!className)
      throw('xbObjects.js:_Classes::registerClass: className missing');

    if (className in _classes)
      return;

    if (className != 'xbObject' && !parentClassName)
      parentClassName = 'xbObject';
    if (!parentClassName)
      parentClassName = null;
    else if ( !(parentClassName in _classes))
      throw('xbObjects.js:_Classes::registerClass: parentClassName ' + parentClassName + ' not defined');

    // evaluating and caching the prototype object in registerClass
    // works so long as we are dealing with 'normal' source files
    // where functions are created in the global context and then
    // statements executed. when evaling code blocks as in xbCOM,
    // this no longer works and we need to defer the prototype caching
    // to the defineClass method

    _classes[className] = { 'classConstructor': null, 'parentClassName': parentClassName };
  }
  _Classes.prototype.registerClass = registerClass;

  function defineClass(className, prototype_func)
  {
    var p;

    if (!className)
      throw('xbObjects.js:_Classes::defineClass: className not given');
    var classRef = _classes[className];
    if (!classRef)
      throw('xbObjects.js:_Classes::defineClass: className ' + className + ' not registered');

    if (classRef.classConstructor)
      return;
    classRef.classConstructor = eval( className );
    var childPrototype  = classRef.classConstructor.prototype;
    var parentClassName = classRef.parentClassName;

    if (parentClassName)
    {
      var parentClassRef = _classes[parentClassName];
      if (!parentClassRef)
        throw('xbObjects.js:_Classes::defineClass: parentClassName ' + parentClassName + ' not registered');

      if (!parentClassRef.classConstructor)
      {
        // force parent's prototype to be created by creating a dummy instance
        // note constructor must handle 'default' constructor case
        var dummy;
        eval('dummy = new ' + parentClassName + '();');
      }
      var parentPrototype = parentClassRef.classConstructor.prototype;

      for (p in parentPrototype)
      {
        switch (p)
        {
        case 'isa':
        case 'classRef':
        case 'parentPrototype':
        case 'parentConstructor':
        case 'inheritedFrom':
          break;
        default:
          childPrototype[p] = parentPrototype[p];
          break;
        }
      }
    }

    prototype_func();

    childPrototype.isa        = className;
    childPrototype.classRef   = classRef;

    // cache method implementor info
    childPrototype.inheritedFrom = new Object();
    if (parentClassName)
    {
      for (p in parentPrototype)
      {
        switch (p)
        {
        case 'isa':
        case 'classRef':
        case 'parentPrototype':
        case 'parentConstructor':
        case 'inheritedFrom':
          break;
        default:
          if (childPrototype[p] == parentPrototype[p] && parentPrototype.inheritedFrom[p])
          {
            childPrototype.inheritedFrom[p] = parentPrototype.inheritedFrom[p];
          }
          else
          {
            childPrototype.inheritedFrom[p] = parentClassName;
          }
          break;
        }
      }
    }
  }
  _Classes.prototype.defineClass = defineClass;
}

// create global instance
var _classes = new _Classes();

// register root class xbObject
_classes.registerClass('xbObject');

function xbObject()
{
  _classes.defineClass('xbObject', _prototype_func);

  this.init();

  function _prototype_func()
  {
    // isa is set by defineClass() to the className
    // Note that this can change dynamically as the class is cast
    // into it's ancestors...
    xbObject.prototype.isa        = null;
    // classref is set by defineClass() to point to the
    // _classes entry for this class. This allows access
    // the original _class's entry no matter how it has
    // been recast.
    // *** This will never change!!!! ***
    xbObject.prototype.classRef      = null;
    xbObject.prototype.inheritedFrom = new Object();

    function init() { }
    xbObject.prototype.init        = init;
    function destroy() {}
    xbObject.prototype.destroy      = destroy;

    function parentMethod(method, arg1, arg2, arg3, arg4, arg5, arg6, arg7, arg8, arg9, arg10)
    {
      // find who implemented this method
      var className       = this.isa;
      var parentClassName = _classes[className].classConstructor.prototype.inheritedFrom[method];
      var tempMethod      = _classes[parentClassName].classConstructor.prototype[method];
      // 'cast' this into the implementor of the method
      // so that if parentMethod is called by the parent's method,
      // the search for it's implementor will start there and not
      // cause infinite recursion
      this.isa   = parentClassName;
      var retVal = tempMethod.call(this, arg1, arg2, arg3, arg4, arg5, arg6, arg7, arg8, arg9, arg10);
      this.isa   = className;

      return retVal;
    }
    xbObject.prototype.parentMethod    = parentMethod;

    function isInstanceOf(otherClassConstructor)
    {
      var className = this.isa;
      var otherClassName = otherClassConstructor.prototype.isa;

      while (className)
      {
        if (className == otherClassName)
          return true;

        className = _classes[className].parentClassName;
      }

      return false;
    }
    xbObject.prototype.isInstanceOf    = isInstanceOf;
  }
}

// eof: xbObjects.js
//</SCRIPT>
```
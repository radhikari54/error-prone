---
title: JavaxInjectOnFinalField
layout: bugpattern
category: INJECT
severity: ERROR
maturity: EXPERIMENTAL
---

<div style="float:right;"><table id="metadata">
<tr><td>Category</td><td>INJECT</td></tr>
<tr><td>Severity</td><td>ERROR</td></tr>
<tr><td>Maturity</td><td>EXPERIMENTAL</td></tr>
</table></div>

# Bug pattern: JavaxInjectOnFinalField
__@javac.inject.Inject cannot be put on a final field.__

## The problem
According to the JSR-330 spec, the @javax.inject.Inject annotation cannot go on final fields.)

## Suppression
Suppress false positives by adding an `@SuppressWarnings("JavaxInjectOnFinalField")` annotation to the enclosing element.

----------

# Examples
__InjectJavaxInjectOnFinalFieldNegativeCases.java__

{% highlight java %}
/*
 * Copyright 2013 Google Inc. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.google.errorprone.bugpatterns;

import javax.inject.Inject;

/**
 * @author sgoldfeder@google.com (Steven Goldfeder)
 */
public class InjectJavaxInjectOnFinalFieldNegativeCases {

  /**
   * Class has no final fields or @Inject annotations.
   */
  public class TestClass1 {
  }

  /**
   * Class has a final field that is not injectable.
   */
  public class TestClass2 {
    public final int n = 0;
  }

  /**
   * Class has an injectable(javax.inject.Inject) field that is not final.
   */
  public class TestClass3 {
    @Inject
    public int n;
  }

  /**
   * Class has an injectable(javax.inject.Inject), final method.
   */
  public class TestClass4 {
    @Inject
    final void method() {}
  }
}

{% endhighlight %}

__InjectJavaxInjectOnFinalFieldPositiveCases.java__

{% highlight java %}
/*
 * Copyright 2013 Google Inc. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.google.errorprone.bugpatterns;

import javax.inject.Inject;

/**
 * @author sgoldfeder@google.com (Steven Goldfeder)
 */
public class InjectJavaxInjectOnFinalFieldPositiveCases {
  
  /**
   * Class has a final injectable(javax.inject.Inject) field.
   */
  public class TestClass1 {
    // BUG: Diagnostic contains: remove
    @Inject 
    public final int n = 0;
  }
}

{% endhighlight %}

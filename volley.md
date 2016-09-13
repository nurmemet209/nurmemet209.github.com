#Volley 基础

* Volley使用缓存

```java


public void addToRequestQueue(Request req, boolean canReadCache) {
        if (canReadCache) {
            //检查网络
            if (!ApplicationUtils.checkDataConnectionState(mContext, false)) {
                if (req instanceof CustomJSonRequest) {
                    CustomJSonRequest request = (CustomJSonRequest) req;
                    if (mRequestQueue.getCache().get(req.getUrl()) != null) {
                        String cacheData = new String(mRequestQueue.getCache().get(req.getUrl()).data);
                        JSONObject obj = JSONObject.parseObject(cacheData);
                        request.getListener().onResponse(obj);
                    }
                }
            } else {
                addToRequestQueue(req);
            }
        } else {
            addToRequestQueue(req);
        }
    }




public void addToRequestQueue(Request req) {
getRequestQueue().add(req);
}




package com.cn.net;

import com.alibaba.fastjson.JSONException;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.parser.Feature;
import com.android.volley.NetworkResponse;
import com.android.volley.ParseError;
import com.android.volley.Request;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.HttpHeaderParser;
import com.cn.util.MyLogger;

import java.io.UnsupportedEncodingException;

/**
 * Created by erqal on 2015/10/10.
 */
public class CustomJSonRequest extends Request<JSONObject> {

    interface OnBackListener {
        void OnBack();
    }

    private OnBackListener onBackListener;
    /**
     * Default charset for JSON request.
     */
    protected static final String PROTOCOL_CHARSET = "utf-8";

    public Response.Listener<JSONObject> getListener() {
        return mListener;
    }

    public void setListener(Response.Listener<JSONObject> mListener) {
        this.mListener = mListener;
    }

    private Response.Listener<JSONObject> mListener;


    public CustomJSonRequest(int method, String url, Response.Listener<JSONObject> listener, Response.ErrorListener errorListener) {
        super(method, url, errorListener);
        this.mListener = listener;
    }

    public CustomJSonRequest(int method, String url, Response.Listener<JSONObject> listener, Response.ErrorListener errorListener, OnBackListener onBackListener) {
        super(method, url, errorListener);
        this.mListener = listener;
        this.onBackListener = onBackListener;
    }

    public CustomJSonRequest(String url, Response.Listener<JSONObject> listener,
                             Response.ErrorListener errorListener) {
        super(Method.GET, url, errorListener);
        this.mListener = listener;
    }


    @Override
    protected Response<JSONObject> parseNetworkResponse(NetworkResponse response) {
        if (onBackListener != null) {
            onBackListener.OnBack();
        }
        String jsonString = "";
        try {
            jsonString = new String(response.data,
                    PROTOCOL_CHARSET);
            return Response.success(JSONObject.parseObject(jsonString, Feature.OrderedField),
                    HttpHeaderParser.parseCacheHeaders(response));
        } catch (UnsupportedEncodingException e) {
            MyLogger.showJson(jsonString);
            return Response.error(new ParseError(e));
        } catch (JSONException je) {
            MyLogger.showJson(jsonString);
            return Response.error(new ParseError(je));
        }
    }

    @Override
    protected void deliverResponse(JSONObject response) {
        mListener.onResponse(response);
    }

    @Override
    public void deliverError(VolleyError error) {
        super.deliverError(error);
    }


}


```

* Volley多次请求导致表单多次提交的问题

如果网速慢的话Volley会自动重新请求，其实这个功能本身是很实用很方便的但是这有的时候会导致有些表单
重复提交的问题，而且开发过程当中也会带来麻烦，因为后台那边调试断点段住导致多次请求，后台调试起来不方便。不过Volley可控制行这方面做的比较好，也提供了可以控制的函数,有些不需要重复请求的请求
可以用以下方法设置为不可重试

```java
req.setRetryPolicy(new DefaultRetryPolicy(15000,//默认超时时间，应设置一个稍微大点儿的，例如本处的500000
        0,//默认最大尝试次数
        DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
getRequestQueue().add(req);
```
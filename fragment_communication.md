#Fragment 之间通信

关于Fragment之间的通信网上基本上也是用接口，用宿主Activity或Fragment，用EventBus等事件库三种，但是
其中用接口通信是有点问题的，是因为当内存不足时Fragment可能销毁重建，重建的时候是用空构造函数来重建，而且这时候宿主可能
没有被销毁重建所以新建的Fragment不会持有原先接口的引用
* 通过宿主通信

MainActivity.java
```java

package com.nurmemet.fragmentcommunication;

import android.support.v4.app.Fragment;
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private MyViewPagerAdapter mAdapter;
    private List<Fragment> mFragmentList;
    private ViewPager mViewPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initViewPager();
    }

    private void initViewPager() {
        mViewPager = (ViewPager) findViewById(R.id.view_pager);
        mFragmentList = new ArrayList<>();
        Fragmen1 fragment1 = new Fragmen1();
        fragment1.setOnResetTitleListener(new ResetTitleListener() {
            @Override
            public void onResetTitle(String title) {
                Fragment2 fragment2 = (Fragment2) mFragmentList.get(1);
                fragment2.setTitle(title);
            }
        });
        mFragmentList.add(fragment1);
        mFragmentList.add(new Fragment2());
        mFragmentList.add(new Fragment3());
        mAdapter = new MyViewPagerAdapter(getSupportFragmentManager(), mFragmentList);
        mViewPager.setAdapter(mAdapter);
        mViewPager.setOffscreenPageLimit(mFragmentList.size());

    }

    public Fragment3 getFragment3(){
        return (Fragment3)mFragmentList.get(2);
    }


}


```


MyViewPagerAdapter.java
```java
package com.nurmemet.fragmentcommunication;

import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;

import java.util.List;

/**
 * Created by nurmemet on 10/12/2016.
 */

public class MyViewPagerAdapter extends FragmentPagerAdapter {

    private List<Fragment> mList;

    public MyViewPagerAdapter(FragmentManager fm, List<Fragment> mList) {
        super(fm);
        this.mList = mList;
    }

    @Override
    public Fragment getItem(int position) {
        return mList.get(position);
    }

    @Override
    public int getCount() {
        return mList.size();
    }
}


```


Fragmen1.java
```java
package com.nurmemet.fragmentcommunication;

import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.EditText;

/**
 * Created by nurmemet on 10/12/2016.
 */

public class Fragmen1 extends Fragment {


    private View mainView;
    private ResetTitleListener mOnResetTitleListener;
    private Button mSendWithInterface;
    private Button mSendWithParent;
    private EditText mContent;

    public void setOnResetTitleListener(ResetTitleListener resetTitleListener) {
        this.mOnResetTitleListener = resetTitleListener;
    }


    public Fragmen1() {

    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        mainView = inflater.inflate(R.layout.fragment_1, null, false);
        mContent = (EditText) mainView.findViewById(R.id.content);
        mSendWithInterface = (Button) mainView.findViewById(R.id.sender);
        mSendWithInterface.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mOnResetTitleListener != null) {
                    mOnResetTitleListener.onResetTitle(mContent.getText().toString());
                }
            }
        });
        mSendWithParent = (Button) mainView.findViewById(R.id.sender_with_parent);
        mSendWithParent.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                getFragment3().setTitle(mContent.getText().toString());
            }
        });
        return mainView;
    }

    private Fragment3 getFragment3() {
        return ((MainActivity) getActivity()).getFragment3();
    }

}

```

Fragment2.java
```java
package com.nurmemet.fragmentcommunication;


import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by nurmemet on 10/12/2016.
 */

public class Fragment2 extends Fragment {

    private View mainView;
    private TextView mTitle;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        mainView = inflater.inflate(R.layout.fragment_2, null, false);
        mTitle = (TextView) mainView.findViewById(R.id.title);
        return mainView;
    }

    public void setTitle(String title) {
        mTitle.setText(title);
    }
}

```

```java
package com.nurmemet.fragmentcommunication;

import android.os.Bundle;
import android.support.annotation.Nullable;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

/**
 * Created by nurmemet on 10/12/2016.
 */

public class Fragment3 extends Fragment {
    private View mainView;
    private TextView mTitle;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        mainView = inflater.inflate(R.layout.fragment_3, null, false);
        mTitle= (TextView) mainView.findViewById(R.id.title);

        return mainView;
    }

    public void setTitle(String title) {
        mTitle.setText(title);
    }


}

```


* 通过EventBus 通信
EventBus的官网 